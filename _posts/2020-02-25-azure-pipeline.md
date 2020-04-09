---
title: 使用Azure Pipeline自动构建.net core console应用
description: CI, CD, 使用Azure Pipeline自动构建.net core console应用
categories: [运维]
layout: main
---

## {{ page. title }}
{{ page.date | date_to_string }}

Azure Pipeline中的task名称和一般build的命令工具是对应的，对于.net平台，常用于编译和发布的命令是`dotnet`, 而于这个命令对于的task名称为DotNetCoreCLI@2，其中@2是当前版本。

例如，`dotnet build`命令：
```cmd
dotnet build --configuration Release
```

使用Azure Pipeline来构建一个.net core console应用程序，我们需要执行以下任务：
1. dotnet restore
1. donet build
1. donet publish
1. 将build artifacts放到drop位置

对于.net core console应用，我们期待输出`.exe`文件, 在Azure Pipeline中如果仅仅执行`dotnet build`命令，它只会生成`.dll`文件，而不会生成`.exe`文件。需要生成可运行的`.exe`文件，就需要给`publish`命令加入以下参数：
```
--configuration Release --self-contained -r win10-x64 --output $(Build.ArtifactStagingDirectory)/Release
```
`--self-contained -r win10-x64`参数将会在build中生成`.exe`的文件。
需要注意的是，这时候如果publish有这个参数`--no-build`，publish任务就会失败。错误信息如下：
> ##[error]Error: The process '/opt/hostedtoolcache/dotnet/dotnet' failed with exit code 1

接下来，我们要在`publish`中设置启用ZIP格式，这个参数将帮我们把输出的文件压缩为一个ZIP包放到drop位置。

```
zipAfterPublish: true
```

完成ZIP包的生成以后，我们需要将ZIP文件放到drop位置，使用`$(Build.ArtifactStagingDirectory)`作为发布的路径。

对于.net core console应用，完整的restore, build, publish, test命令如下：

```yaml
steps:
  - task: UseDotNet@2
    displayName: 'Use .NET Core sdk 3.1.100'
    inputs:
      packageType: sdk
      version: 3.1.100
  
  - task: DotNetCoreCLI@2
    displayName: 'Restore project dependencies'
    inputs:
      command: 'restore'
      projects: '**/*.csproj'

  - task: DotNetCoreCLI@2
    displayName: 'Build the project - Release'
    inputs:
      command: 'build'
      arguments: '--no-restore --configuration Release'
      projects: '**/*.csproj'

  - task: DotNetCoreCLI@2
    displayName: 'Publish the project - Release'
    inputs:
      command: 'publish'
      projects: '**/*.csproj'
      publishWebProjects: false
      arguments: '--configuration Release --self-contained -r win10-x64 --output $(Build.ArtifactStagingDirectory)/Release'
      zipAfterPublish: true
  
  - task: DotNetCoreCLI@2
    displayName: 'Run unit tests - Release'
    inputs:
      command: 'test'
      arguments: '--no-build --configuration Release'
      publishTestResults: true
      projects: '**/UnitTest*.csproj' 

  - task: PublishBuildArtifacts@1
    displayName: 'Publish Artifact: drop'
    condition: succeeded()
```

`projects` 参数中使用"\*\*/*.csproj"来匹配所有的C#项目。其中"\*\*"来匹配全目录，"\*.csproj"匹配所有".csproj"的项目文件.
