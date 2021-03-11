---
title: Process - 使用Azure DevOps Services REST API 6.0编辑Boards Process
description: Process - 使用Azure DevOps Services REST API 6.0编辑Boards Process
categories: [Azure DevOps]
layout: main
---

## {{ page. title }}
{{ page.date | date_to_string }}

这篇文章手把手演示在Azure DevOps Boards中给`Text (single line)`字段添加watermark属性（即占位符水印）。

效果：  
![样例数据]({{ site.baseurl }}/assets/media/2021-02-23-FieldControl-watermark.png)

在本文章中， 我们使用Postman客户端， 通过[Azure DevOps Rest API](https://docs.microsoft.com/en-us/rest/api/azure/devops/processes/system%20controls/update?view=azure-devops-rest-6.0)来编辑Azure DevOps Process。  

## **步骤一：获取Personal Access Token**  
> 略  
## **步骤二：在Postman中，使用PAT进行身份认证**  
**设置Authorization权限.**
1. 在Type选择框中，选中Basic Auth.
1. Username: 留空
1. Password: 填入你自己的Azure DevOps中的Personal Access Token （确保Token对将要操作的organization有足够权限）
## **步骤三：使用到的Azure DevOps Rest API及步骤**
1. **List Process**, 获取某个组织下定义的Process， 取得一个Process的`processId`.
    ```API
    GET https://dev.azure.com/{organization}/_apis/work/processes?$expand=All&api-version=6.0-preview.2
    ```
1. **List Work Item Types**, 通过ProcessId获取这个Process下所有Work Item Type定义, 取得一个Work Item Type的`witRefName`.
    ```API
    GET https://dev.azure.com/{organization}/_apis/work/processes/{processId}/workitemtypes?$expand=all&api-version=6.0-preview.2
    ```
1. **List Layout of a specific work item type**, 通过ProcessId, witRefName获取这个Work Item Type的layout定义， 取得将要更新的控件所在的`groupId`, `controlId`.
    ```API
    GET https://dev.azure.com/{organization}/_apis/work/processes/{processId}/workItemTypes/{witRefName}/layout?api-version=6.0-preview.1
    ```

    **Sample Response**

    ```json
    {
        "pages": [
            {
                "id": "d0171d51-ff84-4038-afc1-8800ab613160",
                "inherited": true,
                "overridden": true,
                "label": "Details",
                "pageType": "custom",
                "visible": true,
                "isContribution": false,
                "sections": [
                    {
                        "id": "Section1",
                        "groups": [                        
                            {
                                "id": "f2bd35c8-c82f-43c1-b79e-06323373cf92",
                                "label": "Custom",
                                "isContribution": false,
                                "visible": true,
                                "controls": [                                
                                    {
                                        "id": "Custom.ControlName",
                                        "label": "{Your Control Name}",
                                        "controlType": "FieldControl",
                                        "readOnly": false,
                                        "visible": true,
                                        "isContribution": false
                                    }
                                ]
                            }
                        ]
                    }]
            }        
        ],
        "systemControls": [
            {
                "id": "System.Title",
                "label": "",
                "controlType": "FieldControl",
                "readOnly": false,
                "watermark": "Enter title",
                "visible": true,
                "isContribution": false
            }
        ]
    }
    ```

1. **Modify the 'watermark' property of a specific control**, 使用`Patch`方法更新Control的属性。
    ```API
    PATCH https://dev.azure.com/{organization}/_apis/work/processes/{processId}/workItemTypes/{witRefName}/layout/groups/{groupId}/controls/{controlId}?api-version=6.0-preview.1
    ```
    **body**
    ```json
    {
        "id": "Custom.ControlName",
        "label": "{Your Control Name}",
        "controlType": "FieldControl",
        "readOnly": false,
        "visible": true,
        "isContribution": false,
        "watermark":"Enter value here"
    }
    ```
    在json格式的body中， `watermark`就是要更新的文本框字段占位符水印属性，注意最好的做法：把原来的所有属性值放到json body中， 新添加`watermark`属性并赋值。
