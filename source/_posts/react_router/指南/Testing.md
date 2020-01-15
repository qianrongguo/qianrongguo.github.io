##  Testing

React Router依靠React上下文来工作。这会影响您如何测试使用我们的组件的组件。

### Context

如果您尝试对呈现`<Link>`或`<Route>`等的组件之一进行单元测试，则会收到一些有关上下文的错误和警告。虽然您可能会想自己尝试添加路由器上下文，但是我们建议您将单元测试包装在以下路由器组件之一中：具有历史记录属性的基本路由器，或`<StaticRouter>`，`<MemoryRouter>`或`<BrowserRouter>`（如果window.history在测试环境中可用作全局变量）。

建议使用MemoryRouter或自定义历史记录，以便能够在两次测试之间重置路由器。

```js
class Sidebar extends Component {
  // ...
  render() {
    return (
      <div>
        <button onClick={this.toggleExpand}>expand</button>
        <ul>
          {users.map(user => (
            <li>
              <Link to={user.path}>{user.name}</Link>
            </li>
          ))}
        </ul>
      </div>
    );
  }
}

// broken
test("it expands when the button is clicked", () => {
  render(<Sidebar />);
  click(theButton);
  expect(theThingToBeOpen);
});

//  fixed
test("it expands when the button is clicked", () => {
  render(
    <MemoryRouter>
      <Sidebar />
    </MemoryRouter>
  );
  click(theButton);
  expect(theThingToBeOpen);
});
```

### 从特定路线开始

`<MemoryRouter>`支持initialEntries和initialIndex道具，因此您可以在特定位置启动应用程序（或应用程序的任何较小部分）。
```js
test("current user is active in sidebar", () => {
  render(
    <MemoryRouter initialEntries={["/users/2"]}>
      <Sidebar />
    </MemoryRouter>
  );
  expectUserToBeActive(2);
});
```

### Navigating

我们进行了很多测试，以检查路线在位置更改时是否有效，因此您可能不需要测试这些东西。但是，如果您需要在应用程序中测试导航，则可以这样进行：

```js
// app.js (a component file)
import React from "react";
import { Route, Link } from "react-router-dom";

// our 项目, the App, but you can test any 子项
// 部分 of your app too
const App = () => (
  <div>
    <Route
      exact
      path="/"
      render={() => (
        <div>
          <h1>Welcome</h1>
        </div>
      )}
    />
    <Route
      path="/dashboard"
      render={() => (
        <div>
          <h1>Dashboard</h1>
          <Link to="/" id="click-me">
            Home
          </Link>
        </div>
      )}
    />
  </div>
);
```

```js
// 您也可以使用 a renderer like "@testing-library/react" or "enzyme/mount" here
import { render, unmountComponentAtNode } from "react-dom";
import { act } from 'react-dom/test-utils';
import { MemoryRouter } from "react-router-dom";

// app.test.js
it("navigates home when you click the logo", async => {
  // in a real test a renderer like "@testing-library/react"
  // would 负责设置 the DOM elements
  const root = document.createElement('div');
  document.body.appendChild(root);

  // Render app
  render(
    <MemoryRouter initialEntries={['/my/initial/route']}>
      <App />
    <MemoryRouter>,
    root
  );

  // 页面互动
  act(() => {
    // 查找链接（可能使用文本内容）
    const goHomeLink = document.querySelector('#nav-logo-home');
    // Click it
    goHomeLink.dispatchEvent(new MouseEvent("click", { bubbles: true }));
  });

  // 检查显示的页面内容是否正确
  expect(document.body.textContent).toBe('Home');
});
```

### 检查测试中的位置

您不必在测试中经常访问位置或历史记录对象，但如果这样做（例如，验证是否在url栏中设置了新的查询参数），则可以添加一条路由来更新测试中的变量：

```js
// app.test.js
test("clicking filter links updates product query params", () => {
  let history, location;
  render(
    <MemoryRouter initialEntries={["/my/initial/route"]}>
      <App />
      <Route
        path="*"
        render={({ history, location }) => {
          history = history;
          location = location;
          return null;
        }}
      />
    </MemoryRouter>,
    node
  );

  act(() => {
    // example: click a <Link> to /products?id=1234
  });

  // assert about url
  expect(location.pathname).toBe("/products");
  const searchParams = new URLSearchParams(location.search);
  expect(searchParams.has("id")).toBe(true);
  expect(searchParams.get("id")).toEqual("1234");
});

```

备选方案

   -   如果您的测试环境具有浏览器全局变量window.location和window.history（这是通过JSDOM在Jest中的默认设置，但您无法重置测试之间的历史记录），则也可以使用BrowserRouter。
   -   您可以将基本路由器与历史包中的历史道具一起使用，而不是将自定义路由传递给MemoryRouter

```js
// app.test.js
import { createMemoryHistory } from "history";
import { Router } from "react-router";

test("redirects to login page", () => {
  const history = createMemoryHistory();
  render(
    <Router history={history}>
      <App signedInUser={null} />
    </Router>,
    node
  );
  expect(history.location.pathname).toBe("/login");
});
```

### React测试库
