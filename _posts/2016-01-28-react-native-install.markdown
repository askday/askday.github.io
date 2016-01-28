---
layout: page
title:  "react-native开发环境搭建"
date:   2016-01-28 16:20:00 +05:30
categories: react-native
author: wangxiang
github_repo_username: askday
comments: false
---

1.安装

安装brew

ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

安装nvm 先设置一个.bash_profile文件

curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.30.1/install.sh | bash

安装node js

nvm install node && nvm alias default node

由于墙的关系 该命令无法执行

可到node.js 官网下载https://nodejs.org/en/

最新的安装包,安装完在终端下运行node -v成功即可

brew install watchman

brew install flow

npm install -g react-native-cli

在已有的工程中添加react-native模块，只需在工程的根目录下运行npm intall 或者 npm update

2.开发环境

下载atom：https://atom.io/

添加react-native开发插件 nuclide

git clone https://github.com/facebook/nuclide.git

$ cd nuclide

$ npm install

$ apm link
