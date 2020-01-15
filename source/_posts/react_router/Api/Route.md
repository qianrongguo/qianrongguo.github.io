
##	Route render methods
建议使用`<Route>`渲染某些内容的方法是使用子元素，如上所示。但是，还有一些其他方法可用于使用`<Route>`渲染内容。提供这些主要是为了支持在引入`hook`之前使用早期版本的路由器构建的应用程序。

- `<Route component>`
- `<Route render>`
- ` <Route children> function`
 您应该在给定的<Route>上仅使用这些道具之一。请参阅下面的说明以了解它们之间的区别。
 
 ###	Route props
 所有这三种渲染方法将通过相同的三个路由道具。
 - match
 - location
 - history

 ###  component
 一个仅在位置匹配时才呈现的React组件。它将与路线道具一起渲染。
 
```js
import React from "react";
import ReactDOM from "react-dom";
import { BrowserRouter as Router, Route } from "react-router-dom";

// All route props (match, location and history) are available to User
function User(props) {
  return <h1>Hello {props.match.params.username}!</h1>;
}

ReactDOM.render(
  <Router>
    <Route path="/user/:username" component={User} />
  </Router>,
  node
);```

当您使用组件（而不是下面的渲染器或子组件）时，路由器会使用React.createElement从给定的组件中创建一个新的React元素。这意味着，如果您向组件prop提供内联函数，则将在每个渲染中创建一个新组件。这将导致现有组件的卸载和新组件的安装，而不仅仅是更新现有组件。使用内联函数进行内联渲染时，请使用render或children道具（如下）。

###		render: func
这样可以方便地进行内联渲染和包装，而无需进行上述不必要的重新安装。无需使用组件prop为您创建新的React元素，而是可以传递位置匹配时要调用的函数。渲染道具功能可以访问与组件渲染道具相同的所有路线道具（匹配，位置和历史）。

```js
import React from "react";
import ReactDOM from "react-dom";
import { BrowserRouter as Router, Route } from "react-router-dom";

// convenient inline rendering
ReactDOM.render(
  <Router>
    <Route path="/home" render={() => <div>Home</div>} />
  </Router>,
  node
);

// wrapping/composing
// You can spread routeProps to make them available to your rendered Component
function FadingRoute({ component: Component, ...rest }) {
  return (
    <Route
      {...rest}
      render={routeProps => (
        <FadeIn>
          <Component {...routeProps} />
        </FadeIn>
      )}
    />
  );
}

ReactDOM.render(
  <Router>
    <FadingRoute path="/cool" component={Something} />
  </Router>,
  node
);
```
**警告**：`<Route组件>`优先于`<Route渲染>`，因此请勿在同一`<Route>`中同时使用两者。

###	children: func
有时您需要渲染路径是否与位置匹配。在这种情况下，您可以使用child道具功能。它与render完全一样，除了是否存在匹配项而被调用。

子级渲染道具将接收与组件和渲染方法相同的所有路由道具，除非当路线未能与URL匹配时，则match为null。这使您可以根据路由是否匹配来动态调整UI。如果路线匹配，我们在此处添加一个活动班级。
```js
import React from "react";
import ReactDOM from "react-dom";
import {
  BrowserRouter as Router,
  Link,
  Route
} from "react-router-dom";

function ListItemLink({ to, ...rest }) {
  return (
    <Route
      path={to}
      children={({ match }) => (
        <li className={match ? "active" : ""}>
          <Link to={to} {...rest} />
        </li>
      )}
    />
  );
}

ReactDOM.render(
  <Router>
    <ul>
      <ListItemLink to="/somewhere" />
      <ListItemLink to="/somewhere-else" />
    </ul>
  </Router>,
  node
);
```
这对于动画也可能有用：
```js
<Route
  children={({ match, ...rest }) => (
    {/* Animate will always render, so you can use lifecycles
        to animate its child in and out */}
    <Animate>
      {match && <Something {...rest}/>}
    </Animate>
  )}
/>
```
**警告**：`<Route children>`优先于`<Route component>`和`<Route render>`，因此请不要在同一`<Route>`中使用多个

###	path: string | string[]
path-to-regexp@^1.7.0可以理解的任何有效URL路径或路径数组。
```js
<Route path="/users/:id">
  <User />
</Route>
```
```js
<Route path={["/users/:id", "/profile/:id"]}>
  <User />
</Route>
```
 没有路径的路线总是匹配的。
 
 ### exact: bool
exact:true时，只有在路径与location.pathname完全匹配时才匹配

|  path | location.pathname  |exact   |  matches ?|
| :------------ | :------------ | :------------ | :------------ |
| /one  |   /one/two|   true| no   |
|  /one  | /one/two  |  false |  yes |

###	strict: bool
设置为true时，带有斜杠的路径将只匹配带有斜杠的location.pathname。当location.pathname中有其他URL段时，这无效。
 ```js
 <Route strict path="/one/">
  <About />
</Route>
```
| path  | location.pathname  |  matches? |
| :------------ | :------------ | :------------ |
|/one/   |/one/   | no   |
| /one/  |  /one/ |  yes |
|  /one/ |  /one/two |  yes |

**警告**：strict可以用于强制location.pathname不带斜杠，但是要做到这一点，strict和精确都必须为真。
```js
<Route exact strict path="/one">
  <About />
</Route>
```

| path  | location.pathname  |  matches? |
| :------------ | :------------ | :------------ |
|/one/   |/one/   | yes   |
| /one/  |  /one/ |no |
|  /one/ |  /one/two |  no |

###	location: object
`<Route>`元素尝试将其路径与当前历史记录位置（通常是当前浏览器URL）匹配。但是，也可以传递路径名不同的位置进行匹配。

如需要将`<Route>`匹配到当前历史记录位置以外的位置时，这很有用，如Animated Transitions示例所示。

如果`<Route>`元素包装在`<Switch>`中并且与传递给`<Switch>`的位置（或当前历史记录位置）相匹配，则传递给`<Route>`的位置prop将被<Switch >（在此处给出）。

### sensitive: bool
为true时，如果路径区分大小写，则将匹配。
```js
<Route sensitive path="/one">
  <About />
</Route>
```
|  path | location.pathname  |sensitive   |  matches? |
| :------------ | :------------ | :------------ | :------------ |
| /one  |   /one/two|   true| no   |
|  /one  | /one/two  |  false |  yes |






