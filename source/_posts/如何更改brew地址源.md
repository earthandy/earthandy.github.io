---
title: 如何更改Mac OS的Brew地址源
date: 2022-01-06 01:04:36
keywords: Mac OS,Brew
categories: Mac OS
tags:
  - Brew
---

####  如何更改brew地址源

#####  替换brew.git 仓库地址:

```bash
cd "$(brew --repo)"
git remote set-url origin https://mirrors.aliyun.com/homebrew/brew.git
```

**还原**

```bash
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git
```

#####  替换homebrew-core.git 仓库地址:

```bash
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.aliyun.com/homebrew/homebrew-core.git
```

**还原**

```bash
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git
```

##### 替换homebrew-bottles 访问地址

这个步骤跟你的 macOS 系统使用的 shell 版本有关系,先来查看当前使用的 shell 版本

```bash
echo $SHELL
/bin/zsh
```

1. zsh替换成阿里巴巴的 homebrew-bottles 访问地址:

```bash
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.aliyun.com/homebrew/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```

**还原**

```bash
vi ~/.zshrc
# 然后，删除 HOMEBREW_BOTTLE_DOMAIN 这一行配置
source ~/.zshrc
```

2. bash替换成阿里巴巴的 homebrew-bottles 访问地址:

```bash
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.aliyun.com/homebrew/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile
```

**还原**

```bash
vi ~/.bash_profile
# 然后，删除 HOMEBREW_BOTTLE_DOMAIN 这一行配置
source ~/.bash_profile
```

配置完后再去安装下某些工具就能发现畅快无比了。
