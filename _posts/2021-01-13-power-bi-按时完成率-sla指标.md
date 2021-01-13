---
title: 使用Power BI实现交付及时性SLA指标-按时完成率的计算方法
description: 使用Power BI实现交付及时性SLA指标-按时完成率的计算方法
categories: [报表]
layout: main
---

## {{ page. title }}
{{ page.date | date_to_string }}

## 交付及时性SLA指标-按时完成率的计算方法

一般，交付及时性SLA指标根据两个日期字段计算得出，**目标完成时间**和**实际完成时间**的差额。一个任务是在目标完成时间之前完成的，即为按时完成。实际完成时间在目标完成时间之前完成的任务的比例越多，交付及时性SLA指标就越高。

根据这个计算方法，在Power BI中我们可以自动计算交付及时性SLA指标。我们的样例数据([Test-data.xlsx]({{ site.baseurl }}/assets/media/Test-data.xlsx))件有以下数据，其中使用到的两个时间字段：目标完成时间和实际完成时间。

![样例数据]({{ site.baseurl }}/assets/media/2021-01-13-1-data-source.PNG)

### 在Power BI中将样例数据作为数据源导入
参考步骤：
1. 获取数据 -> 文件 -> Excel， 点击**连接**按钮。
1. 选择Test-data.xlsx文件，点击**打开**按钮。
1. 在导航器中， 选中*Data*表单，点击**加载**按钮完成数据导入。  
    ![加载数据]({{ site.baseurl }}/assets/media/2021-01-13-2-load-data.PNG)
### 编辑数据，添加新建列和新建度量值。
1. 在数据加载完成以后，在Power BI中，切换到**数据**选项卡。  
    ![数据]({{ site.baseurl }}/assets/media/2021-01-13-3-data-pbi.PNG)
1. 选择表工具菜单项，点击**新建列**按钮。
1. 在公式窗口中，使用DAX语言将公式信息修改为：
    ```dax
    提前完成天数 = datediff('Data'[实际完成时间],'Data'[目标完成时间],DAY)
    ```  
    ![DAX语言]({{ site.baseurl }}/assets/media/2021-01-13-5-on-time-delivery-day.PNG)
1. 修改完成后点击**完成**按钮可以看到，数据表中的**提前完成天数**为新添加列。  
    ![添加的计算列]({{ site.baseurl }}/assets/media/2021-01-13-6-new-column.PNG)
1. 选择表工具菜单项，点击**新建度量值**按钮。
1. 在公式窗口中， 使用DAX语言将公式信息修改为：
    ```DAX
    按时完成率 = countrows(filter('Data', 'Data'[提前完成天数]>=0))/count('Data'[任务编号])
    ```
1. 修改完成后点击**完成**按钮。

### 在报表中添加卡片图控件。
1. 在Power BI中， 切换到**报表**选项卡。
1. 在可视化控件中，选择**卡片图**。  
    ![使用卡片图]({{ site.baseurl }}/assets/media/2021-01-13-7-text-card-control.png)
1. 在字段中，选中*按时完成率*并拖动到卡片图属性窗口的**字段**框中。  
    ![选中按时完成率度量值]({{ site.baseurl }}/assets/media/2021-01-13-8-add-field-to-card.PNG)
1. 在报表中，按时完成率**0.94**已经显示在卡片图中。  
    ![按时完成率数值]({{ site.baseurl }}/assets/media/2021-01-13-9-on-time-delivery-sla.PNG)

**注**： 参考其他资料将数值显示为百分比格式或修改卡片图属性进行美化。
