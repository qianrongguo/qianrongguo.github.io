```js
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  Link
} from "react-router-dom";

{/*
本网站共有3页，所有页面均已呈现
 在浏览器中动态显示（不呈现服务器）。
尽管页面永远不会刷新，但请注意
 当您浏览时，React Router使URL保持最新
 通过网站. 保留 the browser history,
 making sure 后退按钮和书签之类的东西
*/} 
export default function BasicExample() {
  return (
    <Router>
      <div>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/about">About</Link>
          </li>
          <li>
            <Link to="/dashboard">Dashboard</Link>
          </li>
        </ul>

        <hr />

        {/*
          <Switch>遍历其所有子节点<Route>
          elements and renders 第一个路径
          matches the current URL. 随时使用 a <Switch> 
          你有很多的routes, but you want 一个一次渲染
        */}
        <Switch>
          <Route exact path="/">
            <Home />
          </Route>
          <Route path="/about">
            <About />
          </Route>
          <Route path="/dashboard">
            <Dashboard />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}

// 你可以将这些组件视为页面
// in your app.

function Home() {
  return (
    <div>
      <h2>Home</h2>
    </div>
  );
}

function About() {
  return (
    <div>
      <h2>About</h2>
    </div>
  );
}

function Dashboard() {
  return (
    <div>
      <h2>Dashboard</h2>
    </div>
  );
}

```

