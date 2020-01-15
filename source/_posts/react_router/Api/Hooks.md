### useHistory
useHistory挂钩使您可以访问可用于导航的历史记录实例。

```jsx harmony
import { useHistory } from "react-router-dom";

function HomeButton() {
  let history = useHistory();
  function handleClick() {
  history.push("/home")
    }
    return  (
    
<button type="button" onClick={handleClick}>
      Go home
 </button>)

}
```

### useLocation
useLocation挂钩返回代表当前URL的位置对象。您可以像useState一样考虑它，只要URL更改，它就会返回一个新位置。

这可能非常有用，例如在您希望每次加载新页面时都使用Web分析工具触发新的“页面浏览”事件的情况下，如以下示例所示：
```jsx harmony
import React from "react";
import ReactDOM from "react-dom";
import {
  BrowserRouter as Router,
  Switch,
  useLocation
} from "react-router-dom";

function usePageViews() {
  let location = useLocation();
  React.useEffect(() => {
    ga.send(["pageview", location.pathname]);
  }, [location]);
}

function App() {
  usePageViews();
  return <Switch>...</Switch>;
}

ReactDOM.render(
  <Router>
    <App />
  </Router>,
  node
);
```

### useParams

useParams返回URL参数的键/值对的对象。使用它来访问当前<Route>的match.params。
```jsx harmony
import React from "react";
import ReactDOM from "react-dom";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  useParams
} from "react-router-dom";

function BlogPost() {
  let { slug } = useParams();
  return <div>Now showing post {slug}</div>;
}

ReactDOM.render(
  <Router>
    <Switch>
      <Route exact path="/">
        <HomePage />
      </Route>
      <Route path="/blog/:slug">
        <BlogPost />
      </Route>
    </Switch>
  </Router>,
  node
);
```

### useRouteMatch

useRouteMatch挂钩尝试以与<Route>相同的方式匹配当前URL。在无需实际呈现`<Route>`的情况下访问匹配数据最有用。

现在，代替
```jsx harmony
import { Route } from "react-router-dom";

function BlogPost() {
  return (
    <Route
      path="/blog/:slug"
      render={({ match }) => {
        // Do whatever you want with the match...
        return <div />;
      }}
    />
  );
}

```

你也可以
```jsx harmony
import { useRouteMatch } from "react-router-dom";

function BlogPost() {
  let match = useRouteMatch("/blog/:slug");

  // Do whatever you want with the match...
  return <div />;
}
```
