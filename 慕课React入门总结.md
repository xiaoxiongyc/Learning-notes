###慕课 React入门总结
>最近在慕课网听了React入门的项目，写篇文章小结一下，为这个课程画上一个句号。

####1.预备知识

>这个课程的内容是用`React`写一个新闻网页，我们用`npm`来管理项目，且使用`webpack`模块打包机进行打包。

**1.1 关于`npm`**

`npm`通常称为`node`包管理器。顾名思义，它的主要功能就是管理`node`包，包括：安装、卸载、更新、查看、搜索、发布等；

在进行此项目之前，我有一定的`npm`基础，所以在做项目的过程中，没有遇到太多问题。想了解`npm`的小伙伴可以参考下面两篇文章：

- [安装Node.js和npm](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/00143450141843488beddae2a1044cab5acb5125baf0882000)
- [NPM小结 - 程序猿小卡](http://www.tuicool.com/articles/VB7nYn)

--

**主要回顾一下`npm`配置国内资源**

> `npm`是一个非常大的`JavaScript`包，但是在国外，所以由于网速的原因，我们使用`npm`进行安装的时候，有时会失败。

还好，淘宝做了一个`npm`的国内镜像。我们安装之后，使用`cnpm install [xxx]` 命令使用淘宝的镜像进行安装了。`cnpm`的安装和使用[淘宝 NPM 镜像](https://npm.taobao.org/)说的很清晰。


另一种方式是直接修改`npm`的配置项`register`，将`npm`所在的位置修改为淘宝的地址，Mac 的做法如下：在命令行中输入如下命令

```
cd ~ //进入根目录
atom .npmrc //用atom打开.npmrc文件
```
将`.npmrc`文件中的内容修改如下：

```
registry = https://registry.npm.taobao.org
```
这样我们使用 `npm install [xxx]` 就是从淘宝镜像进行安装了。

注：关于npm配置国内资源，慕课老师写了一篇文章，讲的很是明白[使用 CNPM 进行 Ionic 环境的安装与配置](http://blog.parryqiu.com/2016/08/18/ionic_installation/)

**1.2 关于`webpack`**

> `webpack`是一个强大的模块打包机，本项目中使用了`React`和`ES6`的语法，所以使用`webpack`是非常合适的呢！

`webpack`的内容还是很多的，比如说配置文件`webpack.config.js`、`loader`的配置等等；我在学习`webpack`的过程中，还是花了很多力气的。想学习`webpack`的小伙伴，我推荐下面这篇文章（真心讲的很好）：

- [入门Webpack，看这篇就够了](http://www.jianshu.com/p/42e11515c10f)

--

**我们主要说说`webpack`的热加载的配置，这个功能真的很好用。**

1. 常规情况下，我们配置好`webpack.config.js`文件之后，使用`webpack`命令进行打包，然后手动刷新页面。就是说，我们在编写代码的过程中，要不断的执行`webpack`命令，再手动刷新，才能看到效果。

2. 还有一种厉害一点的方法，`webpack --watch` 命令来动态监听文件的改变并实时打包，输出新 bundle.js 文件，但是还是要我们手动进行刷新。


3. 最厉害的就是热加载， `webpack-dev-server `主要是启动了一个使用 `express` 的 `Http`服务器 。它的作用主要是用来伺服资源文件 。此外这个 `Http`服务器 和 `client` 使用了 `websocket` 通讯协议，原始文件作出改动后， `webpack-dev-server` 会实时的编译，但是最后的编译的文件并没有输出到目标文件夹，实时编译后的文件都保存到了内存当中。因此使用`webpack-dev-server`进行开发的时候都看不到编译后的文件。

      - 首先安装 `npm install webpack-dev-server`

      - 终端输入命令 `webpack-dev-server --inline`

这样我们在浏览器输入  `http://localhost:8080/` 就能看到我们编写的页面了，并能时时刷新。

**需要注意的是：**

 - `webpack-dev-server`并不能读取你的`webpack.config.js`的配置`output `，你在`webpack.config.js`里面的配置`output`属性是你用`webpack`打包时候才起作用的，对`webpack-dev-server`并不起作用

- `webpack-dev-server`打包生产的文件并不会添加在你的项目目录中。你启动`webpack-dev-server`后，你在目标文件夹中是看不到编译后的文件的,实时编译后的文件都保存到了内存当中，它默认打包的文件名是bundle.js。
- 因此在使用热加载时，我们引入的文件应该是`webpack-dev-server`打包生产的文件(引用`bundle.js`文件需要直接引用根目录下面的！)

    ```
    <script type="text/javascript" src="bundle.js"></script>
    ```
    
这样就能享受时时刷新的待遇啦！

####2.框架

项目中使用`AntDesign`框架来书写样式，`AntDesign`框架的很多组件很好用，比如：`Tabs`、`Form`、`Carousel`、`Modal`、`Menu`、`Button`等；
`AntDesign`的 [官网](https://ant.design/index-cn) 是中文的，学习起来容易一些呢！

`React`项目的重点当然是`React`了，`React`的核心思想是数据驱动，我们从后台取到数据，再将取到的数据用 `this.setState()`进行保存，从而导致页面重新`render`。关于`React`我也在学习的过程中，下面的三篇文献是我最近正在学习的：

- [React 入门实例教程](http://www.ruanyifeng.com/blog/2015/03/react.html)
- [React官网](https://discountry.github.io/react/)
- [React Router 使用教程](http://www.ruanyifeng.com/blog/2016/05/react_router.html?utm_source=tool.lu)

-


阮一峰大神曾说过：真正学会 React 是一个漫长的过程。

一起加油吧！


