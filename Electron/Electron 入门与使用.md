# Electron 入门与使用

> 初识 electron

Electron 是一个使用 JavaScript、HTML 和 CSS 构建桌面应用程序的框架。嵌入 Chromium 和 Node.js 到二进制的 



> 使用脚手架搭建

npm

```
mkdir my-electron-app && cd my-electron-app
npm init
```



Yarn

```
mkdir my-electron-app && cd my-electron-app
yarn init
```



>  执行上述指令后，可在项目初始化配置设置值，但需遵循以下规则：

* `enty point` 应为 `main.js` 
* `author`  与 `description`  可为任意值，但对于**应用打包**是必填项



配置的 package.json 文件应该像这样：

```
{
  "name": "my-electron-app",
  "version": "1.0.0",
  "description": "Hello World!",
  "main": "main.js",
  "author": "Jane Doe",
  "license": "MIT"
}
```



>  将 `electron` 包安装到应用开发的依赖中

npm

```
npm install --save-dev electron
```



yarn

```
yarn add --dev electron
```



> 注意：如果您在安装 Electron 时遇到任何问题，请 参见 [高级安装](https://www.electronjs.org/zh/docs/latest/tutorial/installation) 指南。



最后，希望能够执行 Electron 如下所示，在 package.json 配置文件中的 scripts 字段增加一条 `start` 指令：

```
{
  "scripts": {
    "start": "electron ."
  }
}
```



> 配置main.js 文件

任何 Electron 应用程序的入口都是 `main` 文件。 这个文件控制了**主进程**，它运行在一个完整的 Node.js 环境中，负责控制您应用的生命周期，显示原生界面，执行特殊操作并管理渲染器进程

执行期间，Electron 将依据应用中 `package.json`配置下 `main` 字段中配置的值查找此文件，应该在 [应用脚手架](https://www.electronjs.org/zh/docs/latest/tutorial/quick-start#scaffold-the-project)步骤中配置

初始化这个`main`文件，需要在您项目的根目录下创建一个名为 `main.js` 的空文件

```js
const { BrowserWindow, app, ipcMain } = require("electron");

const path = require("path");

// 添加一个 createWindow() 方法将 index.html 加载进一个新的 BrowseWindow 实例
const createWindow = () => {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      // __dirname: 字符串指向当前正在执行脚本的路径(当前例子中，指向项目的根文件夹)
      // path.join: API 将多个路径联结在一起，创建一个跨平台路径字符串
      preload: path.join(__dirname, "preload.js"),
    },
  });

  win.loadFile("index.html");
};

// 调用 createWindow() 函数打开窗口
app.whenReady().then(() => {
  // 在主进程设置 handle 监听器
  ipcMain.handle("ping", () => "pong");

  createWindow();

  // 关闭所有窗口时退出应用 widows & Linux
  app.on("window-all-closed", () => {
    if (process.platform !== "darwin") {
      app.quit();
    }
  });

  // 如果没有窗口打开则打开另一个窗口 macOS
  app.on("activate", () => {
    if (BrowserWindow.getAllWindows().length === 0) {
      createWindow();
    }
  });
});

```



> 新建 index.html 文件

在可以为我们的应用创建窗口前，我们需要先创建加载进该窗口的内容。 在 Electron 中，各个窗口显示的内容可以是本地 HTML 文件，也可以是一个远程 url 

教程中，将采用本地 HTML 的方式。 在项目根目录下创建一个名为 `index.html` 的文件：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>关于 recat electron</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    <h2>你好，世界！</h2>
  </body>
</html>

```



> 添加预加载脚本从渲染器访问 Node.js

最后，要做的是输出 Electron 的版本号和其依赖项到 web 页面上

主进程中通过 Node 全局 `process` 对象访问这个信息是微不足道的，但是不能直接在主进程中编辑 DOM ，因为它无法访问渲染器文档上下文，它们存在完全不同进程！

这是将预加载脚本连接到渲染器派上用场的地方，预加载脚本在渲染器进程加载之前加载，并有权访问两个渲染器全局（例如 Window 和 document ）和 Node.js 环境

创建一个名为 preload.js 新脚本如下：

```js
const { contextBridge, ipcRenderer } = require("electron");

// 通过预加载脚本从渲染器访问 Node.js
window.addEventListener("DOMContentLoaded", () => {
  const replaceText = (selector, text) => {
    const element = document.getElementById(selector);

    if (element) {
      element.innerText = text;
    }
  };

  for (const dependency of ["chrome", "node", "electron"]) {
    // 访问到 Node.js process.versions 对象，运行一个基本的 replaceText 辅助函数将版本号插入到 HTML 文档
    replaceText(`${dependency}-version`, process.version[dependency]);
  }
});

// 使用预加载脚本
contextBridge.exposeInMainWorld("version", {
  node: () => process.version.node,
  chrome: () => process.version.chrome,
  electron: () => process.version.electron,
  // 向渲染器添加一个叫 ping() 全局函数演示进程间通信
  ping: () => ipcRenderer.invoke("ping"),
  // 除函数外，我们也可以暴露变量
});

```



> 运行 `start` 命令打开应用

npm

```
npm start
```



yarn

```
yarn start
```

