## 快速开始

		npm install -g create-react-app 
		
全局安装create-react-app	create-react-app可以很方便，快速的帮你搭建react app。
		
		npx create-react-app my-app && cd my-app	 //	2.	创建一个react应用项目
		或	npm init react-app my-app 	或	yarn create react-app my-app
 2. 您可以使用npm或yarn从公共npm注册表中安装React Router。
由于我们正在构建网络应用程序，因此在本指南中将使用react-router-dom。
			
		npm install react-router-dom		

 4. 第一个示例：	基本路由。
 		在此示例中，路由器处理了3个“页面”：主页，“关于”页面和“用户”页面。当您单击不同的<Link>时，路由器将呈现匹配的<Route>。
 		注意：在幕后，<Link>会使用真实的href渲染<a>
 		
```js
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  Link
} from "react-router-dom";

export default function App() {
  return (
    <Router>
      <div>
        <nav>
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/about">About</Link>
            </li>
            <li>
              <Link to="/users">Users</Link>
            </li>
          </ul>
        </nav>

        {/* <Switch>通过子<Route> 查找
		呈现与当前URL匹配的第一个。如果有两个相同的URL只渲染一次这个路径不会渲染第二次*/}
        <Switch>
          <Route path="/about">
            <About />
          </Route>
          <Route path="/users">
            <Users />
          </Route>
          <Route path="/">
            <Home />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}
function Home() {
  return <h2>Home</h2>;
}

function About() {
  return <h2>About</h2>;
}

function Users() {
  return <h2>Users</h2>;
}
```

第二个示例：
	这个例子现实的是嵌套路由的方式。路由	'/topic'	加载Topics组件，该组件将在path:	id值上有条件地呈现任何其他	Route
```js
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  Link,
  useRouteMatch,
  useParams
} from "react-router-dom";

export default function App() {
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
            <Link to="/topics">Topics</Link>
          </li>
        </ul>

        <Switch>
          <Route path="/about">
            <About />
          </Route>
          <Route path="/topics">
            <Topics />
          </Route>
          <Route path="/">
            <Home />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}
function Home() {
  return <h2>Home</h2>;
}

function About() {
  return <h2>About</h2>;
}
function Topics() {
  let match = useRouteMatch();
//相对于父路由建立<Route>,而url允许建立相对的links.
  return (
    <div>
      <h2>Topics</h2>

      <ul>
        <li>
          <Link to={`${match.url}/components`}>Components</Link>
        </li>
        <li>
          <Link to={`${match.url}/props-v-state`}>
            Props v. State
          </Link>
        </li>
      </ul>
      {/*Topics页面有自己的<Switch>里面又包含很多的<Route>,Topics里的<Switch>里面的路径是建立在'/topics'路径上的文件。第二个<Route>作为所有主题的页面或者没有主题时选择的页面   */}
      <Switch>
        <Route path={`${match.path}/:topicId`}>
          <Topic />
        </Route>
        <Route path={match.path}>
          <h3>Please select a topic.</h3>
        </Route>
      </Switch>
    </div>
  );
}

function Topic() {
  let { topicId } = useParams();
  return <h3>Requested topic ID: {topicId}</h3>;
}

```
