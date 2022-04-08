---
title: A Deep Dive into use Node js third-party Packages
summary: A Deep Dive into use Node js third-party Packages
date: 2022-02-07 21:41:59
keywords: Node.js,NPM,third-party,Packages,RunKit,Snyk,github
categories: Node.js
tags:
  - NPM
  - Node.js
  - RunKit
  - Snyk
  - github
---

### What is a Package?

A package in Node.js contains all the files you need for a module.

Modules are JavaScript libraries you can include in your project.

### Download a Package
Downloading a package is very easy.

Open the command line interface and tell NPM to download the package you want.

I want to download a package called [follow-redirects](https://www.npmjs.com/package/follow-redirects):

```bash
npm i follow-redirects
```

### Test the Package On [Runkit](https://npm.runkit.com/)

```javascript
var followRedirects = require("follow-redirects")
const { http, https } = require('follow-redirects');
 
http.get('http://bit.ly/900913', response => {
  response.on('data', chunk => {
    console.log(chunk);
  });
}).on('error', err => {
  console.error(err);
});
```

RunKit notebooks completely remove the friction of trying new ideas. With one click you'll have a sandboxed

JavaScript environment where you can instantly switch node versions, use every npm module without having to wait to install it, and even visualize your results. No more configuration, just straight to coding.

###  Something Else Important Maybe You need

####  Find and automatically fix open source vulnerabilities

Use the Snyk Online tools you can get the Package Health Score.

Let get the follow-redirects's Health Score:

![Follow-redirects's vulnerabilities](https://s2.loli.net/2022/02/07/BTkonPMtacC4LRQ.png)

You can get more detail info about package follow-redirects's vulnerabilities on the [Snyk page](https://snyk.io/advisor/npm-package/follow-redirects).

#### Modify the third-party package's Code and use it directly

1. Fork the code to you github
2. Change the code and push it to you github
3. Use this commond `npm install [your github name]/[package name]` in your project

For example, My GitHub is [earthandy](https://github.com/earthandy) and I want to use [hexo-generator-amp](https://github.com/earthandy/hexo-generator-amp) which I forked from [tea3](https://github.com/tea3), just use:

```bash
npm install earthandy/hexo-generator-amp --save
```







