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
```
{
  "scripts": {
    "dev": "nodemon _teemo_/cli.js"
  }
}
```

- 并在 `src` 下创建 `_teemo_` 文件夹 及 `cli` 文件.

```javascript
const { join } = require("path");

const cli = require("/path/to/your/teemo/packages/teemo-cli");

const srcPath = join(__dirname, "..");
const distPath = join(__dirname, "..", "..", "dist");
const inSrc = (filepath) => join(srcPath, filepath);

const serverConfig = cli({
  miniAppEntryDir: srcPath,
  miniAppOutputDir: distPath,
  ignores: [__dirname, inSrc("nodemon.json"), inSrc("package.json")],
});
```


下面需要引入 `teemo` 提供的各种能力.

- teemo-component

`teemo-component` 是 `teemo` 提供的动态化组件.

需要在 `app.json` 进行配置:

```javascript
{
  "usingComponents":{
    "teemo-component": "@teemo/components/teemo-component/index"
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

如果想要将组件放到云端，需要如下配置:

```javascript
- button
  - index.js
  - index.json
  - index.wxml
  - index.wxss

  - teemo.json
```

这里 `teemo.json` 是用来标识这个云端组件的一些信息:

```javascript
{
  "type": "component",
  "bundleName": "my-button"
}
```

如果想使用这个 云端组件:

```html
<teemo-component namespace="myBtn" bundleName="my-btn" props="{{ {value: 'Hello World!'} }}" />

```










