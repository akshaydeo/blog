---
title: Securing Electron app source code
date: 2019-10-28 13:00:46 Z
categories:
- Electron
- Obfuscation
- Craco
- NodeJS
- Terser
- Webpack
layout: post
comments: true
---

[Electron](https://electronjs.org) is one of the best ways for building cross-platform desktop apps. When I was evaluating a toolkit for building [Viwr](https://viwr.app), Electron was a clear winner over nw.js and Qt-based on the community, resources and available libraries.

## 📚 My Electron stack

- Electron : 5.0.4
- ReactJS : 16.4.1

In this codebase, obfuscation and mangling for ReactJS are done by the react-scripts. This blog mainly focuses on obfuscating electron related codebase.

### 🏗 Code structure

I am using ReactJS for writing the front-end part of the Electron app. I have not used any scaffolding scripts for this.

<div class="container">
<div class="row">
<div class="col-md-4 col-sm-12">
<img class="img-responsive" src="/public/images/viwr-code-structure.png" width="100%"/>
</div>
<div class="col-md-8 col-sm-12">
<h4>src</h4>
<code>src</code> is the entry point for all the source code. This folder contains <code>index.js</code> and <code>index.css</code> which are ReactJS specific.

<h4>electrons</h4>
This folder contains all the BrowserWindow`s and corresponding main thread related code. We will be obfuscating files in this folder.

<h4>components</h4>
This folder contains all the <code>ReactJS</code> components.

</div>
</div>
</div>

## 🛡 Securing the source code

Securing the source code is one of the most important steps before releasing any app (unless it's an open-source project). There is very little help available on the web regarding obfuscating and mangling the electron source code.

<div class="container">
<div class="row">
<div class="col-12">
<img class="img-responsive" src="/public/images/config.png" width="100%"/>
</div>
<div class="col-12 text-center" style="font-size:11px;">
<p>Icons by <a href="https://www.glazestock.com/artist/Nermin">Nermin</a> from <a href="https://glazestock.com">glazestock.com</a></p>
</div>
</div>
</div>

### 📦 Tools

#### 1. CRACO

> This is required only if you are using ReactJS for writing your Electron app.

Create React App Configuration Override (CRACO) is an easy and comprehensible configuration layer for create-react-app.

Get all the benefits of create-react-app and customization without using 'eject' by adding a single craco.config.js file at the root of your application and customize your eslint, babel, postcss configurations and many more.

<div class="container text-center" style="margin-bottom:-5px;"><code>craco.config.js</code></div>
```javascript
module.exports = {
  webpack: {
    configure: (webpackConfig, { env, paths }) => {
      // Adding rule for web workers
      webpackConfig.module.rules.push({
        test: /\.worker\.(js|jsx|mjs)$/,
        use: {
          loader: "worker-loader",
          options: {
            fallback: false,
            inline: true
          }
        }
      });
      webpackConfig.target = "electron-renderer";
      webpackConfig["output"]["globalObject"] = "this";
      webpackConfig["optimization"]["noEmitOnErrors"] = false;
      webpackConfig["node"]["fs"] = "empty";
      return webpackConfig;
    }
  }
};
```

This script
- Adds a worker-loader for the renderer thread flow.
- Sets up the target as electron-renderer.
- Enables <code>fs</code> in renderer-thread land.

#### 2. Webpack

An all in one tool for bundling any javascript project. I used a webpack script for merging all my electron main thread codebase into one single object.

<div class="container text-center" style="margin-bottom:-5px;"><code>webpack.config.js</code></div>
```javascript
const path = require("path");
const fs = require("fs");

let node_modules = {};

fs.readdirSync("node_modules")
  .filter(function(x) {
    return [".bin"].indexOf(x) === -1;
  })
  .forEach(function(mod) {
    node_modules[mod] = "commonjs " + mod;
  });

module.exports = {
  mode: "production",
  entry: "./src/electrons/main.js",
  output: {
    pathinfo: true,
    path: path.resolve(__dirname, "build"),
    filename: "electrons.raw.js",
    globalObject: "this"
  },
  externals: node_modules,
  node: {
    __filename: false,
    __dirname: false
  },
  stats: "errors-only",
  target: "node"
};
```

This script
- Combines all the electron main thread code (all the BrowserWindows), and merge them into one single file called <code>electrons.raw.js</code>.
- The obfuscation part for renderer thread is handled by <code>react-scripts</code>. 

#### 3. Terser

A JavaScript parser and mangler/compressor toolkit for ES6+.

```console
terser --compress --drop_console=true --mangle -- build/electrons.raw.js > build/electrons.js
```

This command
- Obfuscated and mangles the single merged file we have created using webpack. The final output looks something like

```javascript
!function(e){var t={};function n(r){if(t[r])return t[r].exports;var i=t[r]={i:r,l:!1,exports:{}};return e[r].call(i.exports,i,i.exports,n),i.l=!0,i.exports}n.m=e,n.c=t,n.d=function(e,t,r){n.o(e,t)||Object.defineProperty(e,t,{enumerable:!0,get:r})},n.r=function(e){"undefined"!=typeof Symbol&&Symbol.toStringTag&&Object.defineProperty(e,Symbol.toStringTag,{value:"Module"}),Object.defineProperty(e,"__esModule",{value:!0})},n.t=function(e,t){if(1&t&&(e=n(e)),8&t)return e;if(4&t&&"object"==typeof e&&e&&e.__esModule)return e;var r=Object.create(null);if(n.r(r),Object.defineProperty(r,"default",{enumerable:!0,value:e}),2&t&&"string"!=typeof e)for(var i in e)n.d(r,i,function(t){return e[t]}.bind(null,i));return r},n.n=function(e){var t=e&&e.__esModule?function(){return e.default}:function(){return e};return n.d(t,"a",t),t},n.o=function(e,t){return Object.prototype.hasOwnProperty.call(e,t)},n.p="",n(n.s=63)}([function(e,t){e.exports=require("tslib")},function(e,t){e.exports=require("electron")},function(e,t,n){"use strict";Object.defineProperty(t,"__esModule",{value:!0});var r=n(34);t.addBreadcrumb=r.addBreadcrumb,t.captureException=r.captureException,t.captureEvent=r.captureEvent,
```

## Script for building the codebase

```shell
RED='\033[0;31m'
GREEN='\033[0;32m'
NC='\033[0m'

rm -r dist || true && rm -r build || true
# To build reactjs
yarn build
if [[ $? -ne 0 ]]; then
    echo "${RED}BUILD FAILED${NC} => $?"
    exit -1
fi
# To bundle the code
yarn bundle
if [[ $? -ne 0 ]]; then
    echo "${RED}BUNDLING FAILED${NC} => $?"
    exit -1
fi
# Obfuscating the code
terser --compress --drop_console=true --mangle -- build/electrons.raw.js > build/electrons.js
if [[ $? -ne 0 ]]; then
    echo "${RED}OBFUSCATION FAILED${NC} => $?"
    exit -1
fi
# Final packaging electron-builder
electron-builder --config.extraMetadata.main=build/electrons.js
resp=$?
echo $resp
if [[ $resp -ne 0 ]]; then
    echo "${RED}PACKING FAILED${NC} => $resp"
    exit -1
fi
```
🙇‍♂️ Thanks 