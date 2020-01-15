```jsx harmony
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  Link
} from "react-router-dom";

// 每个逻辑“路线”都有两个组成部分，一个用于
   //侧边栏，一个用于主区域。 我们想
   //当路径与当前URL匹配。

// 我们将在2中使用此路由配置
   //景点：一次用于侧边栏，一次在主区域
   //内容部分。 所有路线都在同一条
   //将它们显示在<Switch>中的顺序。
const routes = [
  {
    path: "/",
    exact: true,
    sidebar: () => <div>home!</div>,
    main: () => <h2>Home</h2>
  },
  {
    path: "/bubblegum",
    sidebar: () => <div>bubblegum!</div>,
    main: () => <h2>Bubblegum</h2>
  },
  {
    path: "/shoelaces",
    sidebar: () => <div>shoelaces!</div>,
    main: () => <h2>Shoelaces</h2>
  }
];

export default function SidebarExample() {
  return (
    <Router>
      <div style={{ display: "flex" }}>
        <div
          style={{
            padding: "10px",
            width: "40%",
            background: "#f0f0f0"
          }}
        >
          <ul style={{ listStyleType: "none", padding: 0 }}>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/bubblegum">Bubblegum</Link>
            </li>
            <li>
              <Link to="/shoelaces">Shoelaces</Link>
            </li>
          </ul>

          <Switch>
            {routes.map((route, index) => (
              //您可以在许多地方渲染<Route>
                             //如您所愿，在您的应用中。 它将沿着
                             //和其他也与URL匹配的<Route>匹配。
                             //因此，侧边栏或面包屑或其他任何东西
                             //需要您渲染多个事物
                             //在同一网址的多个位置没有任何内容
                             //超过多个<Route>。
              <Route
                key={index}
                path={route.path}
                exact={route.exact}
                children={<route.sidebar />}
              />
            ))}
          </Switch>
        </div>

        <div style={{ flex: 1, padding: "10px" }}>
          <Switch>
            {routes.map((route, index) => (
              // //渲染更多具有相同路径的<Route>
                                //，但这次是不同的组件。
              <Route
                key={index}
                path={route.path}
                exact={route.exact}
                children={<route.main />}
              />
            ))}
          </Switch>
        </div>
      </div>
    </Router>
  );
}

```
