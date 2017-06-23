
##代码分割

单页应用最大特点是不得不让用户使用之前下载整个应用所需的文件。可以考虑代码分割渐进的下载
应用，当然页有其他的工具来解决这个问题，而我们在这篇用户指导中介绍 webpack + bundle loader

><Bundle>是代码分割问题的不错选择，很明显它和路由毫无瓜葛。但当你在一个路由当中时就意味你需要
>渲染一个组件。所以我们可以在用户导航到其他页面时候，让这个组件动态引入进来。这个方案适用于你的
>APP任意部分。

```
import loadSomething from 'bundle?lazy!./Something'

<Bundle load={loadSomething}>
  {(mod) => (
    // do something w/ the module
  )}
</Bundle>
```

作为一个组件，它可以向右侧一样渲染子组件
```
<Bundle load={loadSomething}>
  {(Comp) => Comp
    ? <Comp/>
    : <Loading/>
  )}
</Bundle>
```

