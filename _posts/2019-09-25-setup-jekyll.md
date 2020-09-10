---
layout: main
title: 安装Jekyll
categories: [运维]
---

## {{ page. title }}
{{ page.date | date_to_string }}

### 使用Ubuntu库安装Ruby & Rubygems
```bash
sudo apt update

sudo apt install ruby-full

ruby --version

gem -v
```

### 安装gcc, g++ and make
```bash
sudo apt update

sudo apt install build-essential

gcc --version
```

### 安装bundler gems
```bash
gem install jekyll bundler
```
### 创建Jekyll网站
```bash
jekyll new myblog

cd myblog
```

### Build the site and make it available on local server

```bash
bundle exec jekyll serve

```

### 使用Jekyll语句QnA
1. 使用什么语句可以让pages按照文件名顺序显示？  
使用`sort`关键字，例如： 
```
{% assign labs = site.pages | sort:"name" | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
```
2. 怎样只显示有某个属性定义的文件？  
使用`xxx == nil` 或`xxx !=nil`语句，例如：  
```
{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
{% assign labsContent = labs | where_exp:"page", "page.lab.type == nil" %}
{% assign labsLak = labs | where_exp:"page", "page.lab.type != nil" %}
```
3. 怎样输出一个按照文件名排序的表格？  
使用`for`语句，例如：  
```
{% assign labs = site.pages | sort:"name" | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Module | Lab |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
```

**注**：示例中所用到的头部为：  
```
---
lab:
    title: 'xxx'
    type: 'xxx'
    module: 'xxx'
---
```
