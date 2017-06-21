
##React-router

###第一节 概要

#####前言
[Githup链接](https://github.com/ReactTraining/react-router)
[代码事例](https://reacttraining.com/)

本文章基于`React-router 4.0`


>react-router可应用于浏览器、服务端、native、甚至包括VR，因此许多组件是共享的部分组件适用于特定的环境。
>然而这并不意味你需要在一个环境中安装两个包（例如共享包和特制包），仅需要安装特定环境的包即可。因为共享的
>组件在特定环境的路由包中也会被再次exports。




*这是因为react-router 采用lerna管理，多个包一套代码*

####WEB
```
npm install react-router-dom
# or
yarn add react-router-dom
```

使用方式如下:

`
import {
  BrowserRouter as Router,
  StaticRouter, // for server rendering
  Route,
  Link
  // etc.
} from 'react-router-dom'
`

如果你想让打包文件尽可能的小，可以像如下的代码直接进行模块的引用。理论上Tree-shaking打包器会帮助我们去掉无用的引入，
Webpack2.0中应用了Tree-shaking。


`
import Router from 'react-router-dom/BrowserRouter'
import Route from 'react-router-dom/Route'
// etc.
`

####Native

我们为React-router的原生能力编写了大量文档，推荐阅读此[文档]()

```
yarn add react-router-native
# or if not using the react-native cli
npm install react-router-native
```

所有的模块可以从顶层引入，如下：

```
import {
  NativeRouter as Router,
  DeepLinking,
  AndroidBackButton,
  Link,
  Route
  // etc.
} from 'react-router-native'

```

####不指定环境

```
yarn add react-router
# or if not using the react-native cli
npm install react-router
```
所有的模块可以从顶层引入，如下：

```
import {
  MemoryRouter as Router,
  Route
  // etc.
} from 'react-router'

```
可以随心所欲的在React项目中使用路由导航，导航的状态是存储在路由内存
(***MemoryRouter***),可以查阅NativeRouter的实现方式去了解怎样
集成这个用法到你的项目中去。


###第二节 应用更新阻塞

#####前言
 >React-router有一些位置感知的组件，它们基于location对象决定渲染结果
 >默认情况下，location 是React的上下文模型（***context model***）中被
 >隐式传递的。当location对象发生变化时候，这些组件会从上下文模型中更新location
 >对象，重新渲染。
 
######如下是针对上面问题的例子

我们先来分析这个不能自动更新的组件

```
class UpdateBlocker extends React.PureComponent {
  render() {
    return this.props.children
  }
}
```

当<UpdateBlocker>在装载过程中，任意具有位置感知功能的子组件会根据 location和match对象
去渲染

```
// location = { pathname: '/about' }此时路径
<UpdateBlocker>
  <NavLink to='/about'>About</NavLink>
  // <a href='/about' class='active'>About</a>
  <NavLink to='/faq'>F.A.Q.</NavLink>
  // <a href='/faq'>F.A.Q.</a>
</UpdateBlocker>
```

当路径如下发生变化，<UpdateBlocker>无法探测到prop或者state的变化，所以它的自组件不会
重新渲染
```
// location = { pathname: '/faq' }
<UpdateBlocker>
  // 这个链接不会重新渲染，所以它保留了上一次的激活状态
  <NavLink to='/about'>About</NavLink>
  // <a href='/about' class='active'>About</a>
  <NavLink to='/faq'>F.A.Q.</NavLink>
  // <a href='/faq'>F.A.Q.</a>
</UpdateBlocker>
```

#####shouldComponentUpdate
当路径发生变化，为了让一个组件知道更新去运行 shouldComponentUpdate方法。可以让
shouldComponentUpdate方法具有判断位置变化的能力。

如果你自己实现了shouldComponentUpdate，可以比对当前和下次路径该变化后的context.router
对象。但是作为react使用人员，不应该直接用context。而是不引入context的情况下比较location，
才是最理想的。


 
 
 
 










