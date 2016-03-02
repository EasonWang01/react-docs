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

### 浏览器中的JSX转换

如果你喜欢使用JSX，官方建议使用browserify或webpack或是使用npm下载babel-core，最後在HTML中加入
`<script type="text/babel">`来执行JSX的转换。(先前的babel-browser已移除http://babeljs.io/docs/usage/browser/)

> 注意：
>
> 浏览器中的JSX转换器是相当大的，并且会在客户端导致无谓的计算，这些计算是可以避免的。不要在生产环境使用 - 参考下一节。


### 生产环境：预先编译JSX

如果你有[npm](http://npmjs.org/)，你可以直接运行`npm install -g babel-cli` Babel有内建支持React v0.12+，所有HTML元素会自动转换成对应的`React.createElement(...)`，displayName会自动被加入，这个插件会自动主换所有JSX的文件为javascript的文件，使你可以直接在浏览器中使用，他也会自动去监视档案的变动，并将它自动转换。例如以下指令`babel --watch src/ --out-dir lib/`。

从Babel6开始，预设配置将不再提供转换的功能，你必须手动在执行command时指定，或是从`.babelrc`档案去设定，除此之外还要从npm安装一些babel的其他套件包含`es2015`和`react presets`，更多相关资讯可参考Babel 6发行时所写的Blog。

上述的安装指令范例如下
```
npm install babel-preset-es2015 babel-preset-react

babel --presets es2015,react --watch src/ --out-dir lib/
```
预设情况下，JSX的档案的结尾含有.js将自动被转换，你可以执行`babel --help`查看更多相关资讯。

##范例:
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
