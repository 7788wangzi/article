---
layout: default
title: 安装Jekyll
---

## {{ page. title }}
{{ page.date | date_to_string }}

### 使用Ubuntu库安装Ruby & Rubygems
```
sudo apt update

sudo apt install ruby-full

ruby --version

gem -v
```

### 安装gcc, g++ and make
```
sudo apt update


sudo apt install build-essential

gcc --version
```

### 安装bundler gems
```
gem install jekyll bundler
```
### 创建Jekyll网站
```
jekyll new myblog

cd myblog
```

### Build the site and make it available on local server

```
bundle exec jekyll serve

```
