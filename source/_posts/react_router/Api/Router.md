## Router
所有路由器组件的通用底层接口。通常，应用将使用高级路由器之一代替：
- `<BrowserRouter>`
- `<HashRouter>`
- `<MemoryRouter>`
- `<NativeRouter>`
- `<StaticRouter>`

使用底层`<Router>`的最常见用例是将自定义历史记录与状态管理库（如Redux或Mobx）进行同步。请注意，并不需要将状态管理库与React Router一起使用，它仅用于深度集成。
```js
import React from "react";
import ReactDOM from "react-dom";
import { Router } from "react-router";
import { createBrowserHistory } from "source/_posts/react_router/Api/history";

const history = createBrowserHistory();

ReactDOM.render(
  <Router history={history}>
    <App />
  </Router>,
  node
);
```
### history: object
用于导航的历史对象。
```js
import React from "react";
import ReactDOM from "react-dom";
import { createBrowserHistory } from "history";

const customHistory = createBrowserHistory();

ReactDOM.render(<Router history={customHistory} />, node);
```
### children: node
要渲染的子元素。
```js
 <Router>
  	<App />
</Router>
```
