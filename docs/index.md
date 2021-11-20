# teemo

## 接入方式

- 将原生小程序修改成以下目录:

```
- src
  - pages
  - components
  - app.json
```

- 在 `src` 目录下，执行:

```
npm init -y
```

- 在 `package.json` 添加 scripts:
```json
{
  "scripts": {
    "dev": "NODE_ENV=development nodemon _teemo_/cli.js",
    "prod": "NODE_ENV=production nodemon _teemo_/cli.js"
  }
}
```

- 并在 `src` 下创建 `_teemo_` 文件夹 及 `cli` 文件.

```javascript
const { join } = require("path");
const cli = require("/Users/bibidu/Desktop/projects/teemo/packages/teemo-cli");

const srcPath = join(__dirname, "..");
const distPath = join(__dirname, "..", "..", "dist");
const inSrc = (filepath) => join(srcPath, filepath);

const serverConfig = cli({
  mode: process.env.NODE_ENV,
  requestScope: '@wm/',
  customServerConfig: {
    baseURL: "https://www.dczblindbox.cn",
  },
  miniAppEntryDir: srcPath,
  miniAppOutputDir: distPath,
  ignores: [
    __dirname,
    inSrc("nodemon.json"),
    inSrc("package.json"),
  ],
});

serverConfig &&
  console.log(`[Teemo] Server opening on "${serverConfig.baseURL}".`);
```


下面需要引入 `teemo` 提供的各种能力.

需要在 `app.json` 进行配置:

```javascript
{
  "pages": [
    "@teemo/hosts/teemo-page"
  ],
  "usingComponents":{
    "teemo-component": "@teemo/hosts/teemo-component"
  }
}
```

- app.js

`app.js` 需要初始化 `teemo`

```javascript
const { createApp } = require("@teemo/index");

App({
  onLaunch() {
    createApp(this);
  },
});

```


## teemo-page
`teemo-page` 是动态化的页面容器. 通过 `teemo-page` 可以将编写的原生页面上传至云端而不用占据小程序的体积，另外可以**实时发版**。

### 创建方式
和原生页面的区别是，需要在页面文件夹目录下新建 `teemo.json` 文件:

```json
{
  "type": "page",
  "bundleName": "one-page"
}
```

新建后的目录如下:

```javascript
- one-page
  - index.js
  - index.json
  - index.wxml
  - index.wxss

  - teemo.json
```

### 使用方式
对于 `teemo` 来说，会找寻 `teemo.json` 中 `bundleName=main-page` 的页面来作为首页，所以项目中必须存在这个页面.

如果要跳转到其他动态化页面，需要如下使用:
```javascript
wx.navigateTo({
  url: "/@teemo/hosts/teemo-page?bundleName=factory",
});
```

如上就跳转到了 `bundleName=factory` 的页面.

## teemo-component
`teemo-component` 是动态化的组件容器. 通过 `teemo-component` 可以将编写的原生页面上传至云端而不用占据小程序的体积，另外可以**实时发版**。

### 创建方式
和原生组件的区别是，需要在组件文件夹目录下新建 `teemo.json` 文件:

```json
{
  "type": "component",
  "bundleName": "one-component"
}
```

新建后的目录如下:

```javascript
- one-component
  - index.js
  - index.json
  - index.wxml
  - index.wxss

  - teemo.json
```


### 使用方式
```html
<teemo-component 
  namespace="one-component" 
  bundleName="one-component" 
  props="{{ {value: 'Hello World!'} }}" 
/>
```










