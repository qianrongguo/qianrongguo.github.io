##  Switch

渲染与位置匹配的第一个子元素`<Route>`或`<Redirect>`。

**这与仅使用`<Route>`有什么不同？**
`<Switch>`的独特之处在于它专门呈现一条路由。相反，每个与该位置匹配的`<Route>`都将进行包含性渲染。考虑这些路线。
```js
import { Route } from "react-router";

let routes = (
  <div>
    <Route path="/about">
      <About />
    </Route>
    <Route path="/:user">
      <User />
    </Route>
    <Route>
      <NoMatch />
    </Route>
  </div>
);
```
如果URL是/ about，则`<About>`，`<User>`和`<NoMatch>`将全部呈现，因为它们都与路径匹配。这是设计使然，允许我们以多种方式将	`<Route>`组合到我们的应用中，例如边栏和面包屑，引导程序标签等。

但是，有时我们只选择一个`<Route>`进行渲染。如果我们位于/ about，我们不想同时匹配/：user（或显示“ 404”页面）。使用Switch的方法如下：

```js
import { Route, Switch } from "react-router";

let routes = (
  <Switch>
    <Route exact path="/">
      <Home />
    </Route>
    <Route path="/about">
      <About />
    </Route>
    <Route path="/:user">
      <User />
    </Route>
    <Route>
      <NoMatch />
    </Route>
  </Switch>
);
```
现在，如果我们位于/ about，`<Switch>`将开始寻找匹配的`<Route>`。`<Route path =“ / about” />`将匹配，而`<Switch>`将停止寻找匹配并呈现`<About>`。同样，如果我们在/ michael位置，则会显示`<User>`。

这对于动画过渡也很有用，因为匹配的`<Route>`呈现在与上一个相同的位置。
```js
let routes = (
  <Fade>
    <Switch>
      {/* 这里只有一个child */}
      <Route />
      <Route />
    </Switch>
  </Fade>
);

let routes = (
  <Fade>
    {/* 这里有两个，一个可能会变为null，但会进行过渡解决起来比较麻烦*/}
    <Route />
    <Route />
  </Fade>
);
```

### location: object
用于匹配子元素的位置对象，而不是当前历史记录位置（通常是当前浏览器URL）。

### children: node
`<Switch>`的所有子代应为`<Route>`或`<Redirect>`元素。仅第一个与当前位置匹配的child会被渲染。
`<Route>`元素使用其路径属性进行匹配，而`<Redirect>`元素使用其from属性进行匹配。没有路径属性的`<Route>`或没有from属性的`<Redirect>`将始终与当前位置匹配。
在`<Switch>`中包含`<Redirect>`时，它可以使用`<Route>`的任何位置匹配道具：path, exact, and strict。from只是路径属性的别名。
如果为`<Switch>`提供了位置提示，它将覆盖匹配的子元素上的位置提示。
```js
import { Redirect, Route, Switch } from "react-router";

let routes = (
  <Switch>
    <Route exact path="/">
      <Home />
    </Route>

    <Route path="/users">
      <Users />
    </Route>
    <Redirect from="/accounts" to="/users" />

    <Route>
      <NoMatch />
    </Route>
  </Switch>
);
```
