---
title: 使用Power Automate实现跨organization创建work item并建立remote链接
description: 使用Power Automate实现跨organization创建work item并建立remote链接
categories: [工作流]
layout: main
---

## {{ page. title }}
{{ page.date | date_to_string }}

要实现的一个用户场景是，在同一个active domain中，有2个不同的organization。 在这2个不同的organization之间创建远程链接关系。 例如，在一个org中更新Task任务项，当某种条件满足情况下需要在另一个不同的organization创建一个特定work item type类型的work item。并且创建一个remote dependency链接关系。

### 一种思路是：
1. 先使用`Create a work item`功能在alt org中创建一个新work item.
1. 再使用`Send an Http request to Azure DevOps`创建两个work item之间的链接关系。
    - 当更新一个现有work item时，使用`PATCH`方法。
    - API是 `https://dev.azure.com/{org}/{project}/_apis/wit/workitems/{id}?api-version=6.0`

![先创建，再更新链接]({{ site.baseurl }}/assets/media/create-and-update.png)

### 另一种思路是：
1. 直接使用`Send an HTTP request to Azure DevOps`创建一个新work item，并且建立链接关系。
    - 当创建一个新work item时，使用`POST`方法。
    - 创建work item的API是 `https://dev.azure.com/{org}/{project}/_apis/wit/workitems/${wit}?api-version=6.0&validateOnly=false&bypassRules=true`, 注意，work item type类型前使用$开始，否则报错page not found。

![创建新work item并添加链接]({{ site.baseurl }}/assets/media/create-update.png)

### 更多关于`Send an HTTP request to Azure DevOps`的使用记录

如果是使用`Get`方法来获取一个Work Item的字段，包括关系链接等， 需要对返回的Response进行Json 解析，用到`Parse Json`。

使用`Parse Json`需要2个输入，一个是要解析的文档内容，即`Send an HTTP request to Azure DevOps`的ouput - Body; 另一个是解析时参考的Schema, 这个Schema可以通过使用Postman来手动调用同一个Work item 来获得一个Response， 从Response作为一个Sample产生Schema。

另外， 针对Work Item的操作，我们经常需要从一个Work Item访问到它的Parent Work Item, Get Work Item中有个Parent字段即是Parent work item 的Id.

![Parse json of HTTP Send Request]({{ site.baseurl }}/assets/media/pa-parse-json.png)
