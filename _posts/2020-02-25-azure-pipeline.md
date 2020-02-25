---
layout: main
title: Azure Pipeline
categories: [运维]
---

## {{ page. title }}
{{ page.date | date_to_string }}

Azure Pipeline中的task名称和一般build的命令工具是对应的，对于.net平台，常用于编译和发布的命令是`dotnet`, 而于这个命令对于的task名称为DotNetCoreCLI@2，其中@2是当前版本。

用于编译，发布.net core平台的程序命令为：

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
      arguments: '--no-build --configuration Release --output $(Build.ArtifactStagingDirectory)/Release'
      zipAfterPublish: true
  
  - task: PublishBuildArtifacts@1
    displayName: 'Publish Artifact: drop'
    condition: succeeded()
```