---
title: Power Platform - 创建你的第一个Power Apps应用程序
description: Power Platform - 创建你的第一个Power Apps应用程序
categories: [BizApps, Power Platform]
layout: main
---

## {{ page. title }}
{{ page.date | date_to_string }}

**需求**： 在这篇练习中，我们将使用Onedrive中的Excel表格作为数据源，从0开始实现一个简单手机端app。 App有三个Screen, 分别用作浏览Item, 查看Item的详细信息，增加/修改Item。
1. 使用组织账号登录[Make a Power App](https://make.powerapps.com/).
1. 在Home选项卡, 选择**Canvas app from blank**来创建一个blank app.
1. 在blank app中, 选择Data选项卡, 单击**Add data**来连接数据源, 我们使用Ondrive上的Excel表格作为数据源.
1. 在Tree view中, 将第一个Screen重命名为BrowseScreen.

## 使用Gallery实现BrowseScreen
1. 选择Insert按钮, 选择Gallery, 选择Vertial控件.
1. 在Tree view中, 将新添加的Gallery1重命名为BrowseGallery.
1. 选中BrowseGallery, 在Gallery属性框中, 添加**Data source**, 将Excel中的Table1作为Gallery数据源.
1. 选中BrowseGallery, 在Gallery属性框中, 选中Edit Fields.
1. 在Data属性框中, 为Title和Subtitle选择相应的数据列.

## 添加App Navigation Bar
1. 选择Inseart按钮, 选择Icons, 选择Rectangle控件.
1. 在BrowseScreen中, 将Rectangle控件调整到合适大小并放置到页面最顶端, 可以通过属性框中的Position和Size进行微调.
1. 使用蓝色填充Rectangle的Color属性.
1. BrowseScreen的Navigation Bar就做好了.
1. 适当调整BrowseGallery与Navigation Bar的相对位置.

## 使用Form控件实现DetailScreen
1. 选择New screen, 选择Blank.
1. 在Tree view中, 将新添加的Screen重命名为DetailScreen.
1. 选择Insert按钮, 选择Forms, 选择Edit.
1. 在Tree view中, 将新添加的Form重命名为ViewItemForm.
1. 选择ViewItemForm, 在ViewItemForm的属性窗口中, 设置**Data source**仍然为Table1.
1. 在ViewItemForm的属性窗口中, 选择Edit fields.
1. 在Fields窗口中, 选择**Add field**按钮,将Excel表格中的所有列名选中, 并点击**Add**按钮.
1. 在ViewItemForm的属性窗口中, 将**Default mode**设置为View.
1. 在页面顶部,确保在Item属性中,添加以下功能：
    ```fx
    BrowseGallery.Selected
    ```
1. 将BrowseScreen中添加的Navigation Bar复制到DetailScreen.
1. 适当调整ViewItemForm与Navigation Bar的相对位置.

## 使用Form控件实现EditScreen
1. 选择New screen, 选中Blank.
1. 在Tree view中, 将新添加的Screen重命名为EditScreen.
1. 选择Insert按钮, 选择Forms, 选择Edit.
1. 在Tree view中, 将新添加的Form重命名为EditItemForm.
1. 选择EditItemForm, 在EditItemForm属性窗口中, 设置**Data source**为Table1.
1. 在EidtItemForm的属性窗口中, 选择Edit fields.
1. 在Fields窗口中, 选择**Add field**按钮,将Excell表格中的所有列名选中,并点击**Add**按钮.
1. 在EditItemForm属性窗口中,将**Default mode**设置为Edit.
1. 在页面顶部,确保在Item属性中,添加以下功能：
    ```fx
    BrowseGallery.Selected
    ```
1. 将BrowseScreen中添加的Navigation Bar复制到EditScreen.
1. 适当调整EditItemForm与Navigation Bar的相对位置.

## 在Navigation Bar中添加Screen跳转逻辑

### 添加BrowseScreen与DetailScreen之间的跳转逻辑
1. 在Tree view中, 选中BrowseGallery控件, 选中NextArrow图标.
1. 在页面顶部, 确保在Onselect事件中,添加以下功能：
    ```fx
    Navigate(DetailScreen,ScreenTransition.CoverRight)
    ```
1. 在Tree view中, 选择DetailScreen.
1. 选择Insert, 选择Icons, 选择Left.
1. 将Left icon拖动到Navigation Bar的左上角.
1. 选中Left icon图标, 在页面顶部, 确保在Onselect事件中,添加以下功能：
    ```fx
    Navigate(BrowseScreen,ScreenTransition.CoverRight)
    ```

### 在DetailScreen添加Remove按钮
1. 在Tree view中, 选择DetailScreen.
1. 选择Insert, 选择Icons, 选择Trash.
1. 将新添加的Trash icon拖动到Navigation Bar的相应位置.
1. 选择Trash icon.
1. 在页面顶部, 确保在OnSelect事件中, 添加以下功能：
    ```fx
    Remove(Table1,BrowseGallery.Selected);If (IsEmpty(Errors(Table1,BrowseGallery.Selected)),Back())
    ```

### 在DetailScreen中添加Edit按钮
1. 在Tree view中, 选择DetailScreen.
1. 选择Insert, 选择Icons, 选择Edit.
1. 将新添加的Edit icon拖动到Navigation Bar的相应位置.
1. 选择Edit icon.
1. 在页面顶部, 确保在OnSelect事件中, 添加以下功能：
    ```fx
    EditForm(EditItemForm);Navigate(EditScreen,FormPattern.None)
    ```

### 在EditScreen中添加Save按钮
1. 在Tree view中, 选择EditScreen.
1. 选择Insert, 选择Icons, 选择Save.
1. 将新添加的Save icon拖动到Navigation Bar的相应位置.
1. 选择Save icon.
1. 在页面顶部, 确保在OnSelect事件中, 添加以下功能：
    ```fx
    SubmitForm(EditItemForm)
    ```

### 在EditScreen中添加Cancel按钮
1. 在Tree view中, 选择EditScreen.
1. 选择Insert, 选择Icons, 选择Cancel.
1. 将新添加的Cancel icon拖动到Navigation Bar的相应位置.
1. 选择Cancel icon.
1. 在页面顶部,确保在OnSelect事件中,添加以下功能：
    ```fx
    ResetForm(EditItemForm);Navigate(DetailScreen,ScreenTransition.CoverRight)
    ```

### 在BrowseScreen中添加Add按钮
1. 在Tree view中, 选择BrowseScreen.
1. 选择Insert, 选择Icons, 选择Add.
1. 将新添加的Add icon拖动到Navigation Bar的相应位置.
1. 选择Add icon.
1. 在页面顶部,确保在OnSuccess事件中,添加以下功能：
    ```fx
    NewForm(EditItemForm);Navigate(EditScreen,None)
    ```

### 修改EditScreen和DetailScreen中的Fx支持Add功能
**需求**：当在EditScreen中新增一个Item时,点击Save按钮以后,页面期望导航到DetailScreen并在ViewItemForm中显示新添加的Item. 而不是显示BrowseGallery中选中的Item.
1. 在Tree view中,选择EditItemForm.
1. 在页面顶部,确保在OnSuccess事件中,添加以下功能：
    ```fx
    Set(varLastSubmit, EditItemForm.LastSubmit);Navigate(DetailScreen, ScreenTransition.None)
    ```
**在DetailScreen中修改ViewItemForm的Item属性**
1. 在Tree view中,选择ViewItemForm.
1. 在页面顶部,确保在Item属性中,将属性从`BrowseGallery.Selected`修改为：
```fx
If(!IsBlank(varLastSubmit), varLastSubmit, BrowseGallery.Selected)
```