---
layout: main
title: Azure CLI简介
categories: [Azure]
tags: [Azure, CLI]
---

## Azure CLI是什么

Azure CLI是一个跨平台的命令行工具，用于管理和维护Microsoft Azure上的服务和资源。
类似与PowerShell, Azure CLI提供了从命令行使用和管理Azure服务和资源的方法。您仍然可以像以前一样继续使用PowerShell、API和Azure Portal。
Azure CLI非常灵活，可以快速安装在几乎任何平台上。

## 安装Azure CLI
如何安装Azure CLI取决于你需要在什么平台上使用它， 最简单的方法是在Windows、MacOS、Linux或Docker容器中本地运行它。
您也可以在Windows 10上使用Windows Subsystem for Linux(WSL)。

**安装在Windows**

在Windows中安装Azure CLI，下载[安装包](https://aka.ms/installazurecliwindows)。

**安装在macOS**  

在macOS中安装Azure CLI， 运行如下命令:
```
brew update && brew install azure-cli
```

**安装在Linux**

在Linux上的安装步骤取决于Linux的发行版本，具体指引请参考[这里](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-apt?view=azure-cli-latest).

**安装在WSL（on Windows 10)**

首先，安装WSL，以ubuntu18.04为例，在命令行工具(command prompt)中, 从微软商店下载ubuntu-1804.appx文件。
```
curl.exe -L -o ubuntu-1804.appx https://aka.ms/wsl-ubuntu-1804
```
然后，使用PowerShell启用WSL。
```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```
注意：命令执行完以后需要重启系统。

最后， 在命令行工具中，执行ubuntu-1804.appx的安装。

![install ubuntu-1804.appx](media/install-ubuntu-1804.PNG)

安装完成以后，运行Linux系统，初次运行需要花费时间完成初始化操作。

![init WSL](https://raw.githubusercontent.com/7788wangzi/azure/master/media/init-wsl.PNG)

参照[在Linux中安装Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-apt?view=azure-cli-latest)完成Azure CLI的安装.

**安装在Docker上**

## 在Azure Cloud Shell中使用Azure CLI    

在Azure Cloud Shell中使用Azure CLI，它在Azure Portal中运行。只需打开https://portal.azure.com并单击顶部工具栏中的Cloud Shell图标。

![](https://raw.githubusercontent.com/7788wangzi/azure/master/media/image-25.png)

## 登录Azure账户
```
az login
```

如果在Azure Cloud Shell中使用，则不需要登录。

列出本账户下的所有订阅。

```
az account list --all
```

如果账户下有多个Azure订阅，通过命令切换活动订阅：

```
az account set --subscription "Azure Production"
```

## 格式化输出

在Azure CLI中支持的输出格式：

- json - JSON输出
- jsonc - 有颜色的JSON输出
- yaml - YAML格式输出
- table - 表格格式输出
- tvs - Tab-seperated 输出

例如，尝试输出Azure实例类型:

```
az cloud list
```

输出结果格式化为表格：

```
az cloud list --output table
```

![](https://raw.githubusercontent.com/7788wangzi/azure/master/media/image-29.png)


你也可以通过如下命令修改默认的输出格式:
```
az configure
```

![](https://raw.githubusercontent.com/7788wangzi/azure/master/media/image-30.png)

然后选择一个偏好格式。


## 使用Azure CLI可以做什么

您已经使用Azure CLI成功登录到Azure。实际上，可以使用Azure CLI执行通常使用Azure Portal执行的任何操作。

大多数Azure CLI命令遵循相同的az概念。您可以将命令组替换为数十个选项，例如GROUP(适用于资源组)、VM(适用于虚拟机)、WebApp(适用于Web App)等。

要查看所有可用的命令组，只需键入

```
az
```

### 管理资源组

列出所有资源组
```
az group list
```

创建资源组
```
az group create --name  --location 
```

![](https://raw.githubusercontent.com/7788wangzi/azure/master/media/image-32.png)

删除资源组

```
az group delete --resource-group foo --yes --no-wait
```

--yes 不再提示确认， --no-wait让命令在后台运行

### 使用变量

```
set variable = value
az group create --name foo --location $variable
```

## 使用`find`d命令

有时，您可能不确定将哪个命令用于手头的任务。要搜索命令，请使用：

```
az find --search-query 
```

![](https://raw.githubusercontent.com/7788wangzi/azure/master/media/image-33.png)

## 使用简写
有时你会想缩短你的命令。Azure CLI支持很多缩写，您在第一次使用不同的命令时通常会发现这些缩写。例如，以下命令：

```
az group show --resource-group aci-demo
```

可以被简写为
```
az group show -g aci-demo
```
