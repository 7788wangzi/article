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
