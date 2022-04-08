---
title: Chrome OS install zsh and oh-my-zsh
date: 2021-06-20 22:56:47
keywords: Chrome OS,zsh,on-my-zsh
categories: Chrome OS
tags:
  - Chrome OS
  - Shell
---

### Enable Linux
First things first, we need to enable Linux. Head to Settings > Search 'Linux' and enable the thing. 
### Installing Zsh and Oh My Zsh
Linux on Chrome OS does not ask for a password whenever you use commands with super user access, so we first need to do this:
```bash
sudo su && passwd pushp.vashisht
```
In the same terminal window, we can install Zsh by writing 👇
```bash
sudo apt install zsh && chsh -s $(which zsh)
```
After we have Zsh, we need to install Oh My Zsh, which is a framework for managing Zsh. We already have curl, so just paste this on your terminal:
```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
Type 'Y' to set your Oh My Zsh as your default bash. This command always creates a ~/.zshrc file on your home directory, which we'll alter soon. 
If you struggle like I did to set Zsh as the default prompt, try using the following command
```bash
echo 'exec /usr/bin/zsh' >>~/.bashrc
```
### Installing Powerlevel9k Theme
I use the much lighter Powerlevel 10k Theme, which is also very easy to configure, however I wasn't able to install its required font, Meslo LG S, therefore, we're going with its older cousin P9K, which is a great theme, just a little harder to configure. To install it run this command:
```bash
git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k
```
To change our zsh configuration we need to install a text editor, if you're like me and don't know how to vim, 👉 sudo apt install nano . After this, let's create and change our zsh config and tell it to use our Powerlevel 10k theme. Hit nano ./zshrc and change the ZSH_THEME line to the following 👉 ZSH_THEME="powerlevel9k/powerlevel9k" . Hit Ctrl + X and 'Y' to save and exit.

Press Ctrl + T to open a new terminal window and type zsh . You should already notice some differences.
