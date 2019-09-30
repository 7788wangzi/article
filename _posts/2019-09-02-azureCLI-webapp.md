---
layout: main
title: Azure CLI使用实例-创建Web App
categories: [Azure]
tags: [Azure, CLI, WebApp]
---

## 使用Azure CLI创建webapp

**登录Azure Account**
在命令行工具中运行:
```
az login
```
在弹出的浏览器窗口中输入用户名密码登录。

列出账号下的所有订阅:
```
az account list --output table
```
--output table是以表格形式输出结果。

切换订阅:
```
az account set -s <id>
```

**创建资源组，托管计划和应用程序**  

创建资源组
```
az group create -l westus -n <resource-group name>
```

创建应用程序托管计划
```
az appservice plan create -g <resource-group name> -n <app-plan name>
```

创建应用程序
```
az webapp create -g <resource-group name> -p <app-plan name> -n <webapp name>
```

## 在Azure Portal中设置自动化部署方案

登录Azure Portal。
找到刚刚创建的webapp，在Deployment中选择Deployment Center选项。
![deployment-center](https://raw.githubusercontent.com/7788wangzi/azure/master/media/deployment-center.jpg)

Source Control，选择GitHub。
![github](https://raw.githubusercontent.com/7788wangzi/azure/master/media/github.jpg)

按照指导进行账号认证，在Configure中配置GitHub的Oragnization, Repository, 和Branch。
![github configuration](https://raw.githubusercontent.com/7788wangzi/azure/master/media/github-account-.JPG)

测试更新后自动发布，在gitHub中，修改html文件，返回webapp查看更新是否自动被发布。


[YT 视频](https://youtu.be/HKXtkydR5tA)