---
id: tooling-integration
title: 工具集成（Tooling Integration）
permalink: tooling-integration.html
prev: more-about-refs.html
next: addons.html
---

每个项目使用不同的系统来构建和部署JavaScript。我们尝试尽量让React环境无关。

## React

### CDN托管的React

我们在我们的[下载页面](https://facebook.github.io/react/downloads.html)提供了React的CDN托管版本。这些预构建的文件使用UMD模块格式。直接简单地把它们放在`<script>`标签中将会给你环境的全局作用域引入一个`React`对象。React也可以在CommonJS和AMD环境下正常工作。


### 使用主分支

我们在[GitHub仓库](https://github.com/facebook/react)的主分支上有一些构建指令。我们在`build/modules`下构建了符合CommonJS模块规范的树形目录，你可以放置在任何环境或者使用任何打包工具，只要支持CommonJS规范。

## JSX

### 瀏覽器中的JSX轉換

如果你喜歡使用JSX，官方建議使用browserify或webpack或是使用npm下載babel-core，最後在HTML中加入
`<script type="text/babel">`來執行JSX的轉換。(先前的babel-browser已移除http://babeljs.io/docs/usage/browser/)

> 注意：
>
> 浏览器中的JSX转换器是相当大的，并且会在客户端导致无谓的计算，这些计算是可以避免的。不要在生产环境使用 - 参考下一节。


### 生產環境：預先編譯JSX

如果你有[npm](http://npmjs.org/)，你可以直接運行`npm install -g babel-cli` Babel有內建支持React v0.12+，所有HTML元素會自動轉換成對應的`React.createElement(...)`，displayName會自動被加入，這個插件會自動主換所有JSX的文件為javascript的文件，使你可以直接在瀏覽器中使用，他也會自動去監視檔案的變動，並將它自動轉換。例如以下指令`babel --watch src/ --out-dir lib/`。

從Babel6開始，預設配置將不再提供轉換的功能，你必須手動在執行command時指定，或是從`.babelrc`檔案去設定，除此之外還要從npm安裝一些babel的其他套件包含`es2015`和`react presets`，更多相關資訊可參考Babel 6發行時所寫的Blog。

上述的安裝指令範例如下
```
npm install babel-preset-es2015 babel-preset-react

babel --presets es2015,react --watch src/ --out-dir lib/
```
預設情況下，JSX的檔案的結尾含有.js將自動被轉換，你可以執行`babel --help`查看更多相關資訊。

##範例:
```
$ cat test.jsx
var HelloMessage = React.createClass({
  render: function() {
    return <div>Hello {this.props.name}</div>;
  }
});

ReactDOM.render(<HelloMessage name="John" />, mountNode);
$ babel test.jsx
"use strict";

var HelloMessage = React.createClass({
  displayName: "HelloMessage",

  render: function render() {
    return React.createElement(
      "div",
      null,
      "Hello ",
      this.props.name
    );
  }
});

ReactDOM.render(React.createElement(HelloMessage, { name: "John" }), mountNode);
```

### 有用的开源项目

开源社区开发了在几款编辑器中集成JSX的插件和构建系统。点击[JSX集成](https://github.com/facebook/react/wiki/Complementary-Tools#jsx-integrations)查看所有内容。
