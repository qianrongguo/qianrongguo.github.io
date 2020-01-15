```jsx harmony
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  Link,
  Redirect,
  useParams,
  useRouteMatch
} from "react-router-dom";

/*
有时您不知道所有可能的路线
预先为您的应用程序； 例如，当
构建文件系统浏览UI或确定
网址是根据数据动态生成的。 在这些情况下，
它有助于拥有一个能够
在运行时根据需要生成路由。


该示例使您可以深入了解朋友
递归列表，查看每个用户的朋友列表
一路上。 向下钻取时，请注意每个细分
被添加到URL。 您可以复制/粘贴此链接
  给其他人，他们将看到相同的用户界面。

然后单击后退按钮并观看最后一个
URL的段与最后一个段一起消失
好友列表。

*/


export default function RecursiveExample() {
  return (
    <Router>
      <Switch>
        <Route path="/:id">
          <Person />
        </Route>
        <Route path="/">
          <Redirect to="/0" />
        </Route>
      </Switch>
    </Router>
  );
}

function Person() {
  let { url } = useRouteMatch();
  let { id } = useParams();
  let person = find(parseInt(id));

  return (
    <div>
      <h3>{person.name}’s Friends</h3>

      <ul>
        {person.friends.map(id => (
          <li key={id}>
            <Link to={`${url}/${id}`}>{find(id).name}</Link>
          </li>
        ))}
      </ul>

      <Switch>
        <Route path={`${url}/:id`}>
          <Person />
        </Route>
      </Switch>
    </div>
  );
}

const PEEPS = [
  { id: 0, name: "Michelle", friends: [1, 2, 3] },
  { id: 1, name: "Sean", friends: [0, 3] },
  { id: 2, name: "Kim", friends: [0, 1, 3] },
  { id: 3, name: "David", friends: [1, 2] }
];

function find(id) {
  return PEEPS.find(p => p.id === id);
}

```
