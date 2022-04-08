---
title: How to install Node.js and NPM on a Chromebook
date: 2021-06-21 00:41:51
keywords: Chromebook,Node.js,NPM
categories: Chrome OS
tags:
  - NPM
  - Node.js
---

### Installing Node.js
Download the Node.js from this  [website](https://nodejs.org/)
Navigate to the Downloads folder from the command line:
```bash
cd ~/Downloads
```
Extract the downloaded node package:
```bash
tar xf node*.tar.xz
```
Navigate to the extracted bin file:
```bash
cd node*
cd bin
```
Copy the node file under/usr/local/bin/ (create the bin folder if it doesn’t exist):
```bash
cp node /usr/local/bin/
```
You should be able to see node version now:
```bash
node -v
```
eg.: v8.11.2

### Installing npm
Now navigate to the npm folder:
```bash
cd ..
cd lib/node_modules/npm/scripts
```
And install npm:
```bash
sudo sh install.sh
```
Verify that you have npm installed:
```bash
npm -v
```
eg.: 6.0.1
