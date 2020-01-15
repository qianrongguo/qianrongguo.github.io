### 滚动还原

在早期版本的React Router中，我们提供了对滚动恢复的开箱即用的支持，从那以后人们一直在要求它。希望本文档可以帮助您从滚动条和路由中获得所需的信息！


浏览器开始以自己的history.pushState处理滚动还原，其处理方式与使用普通浏览器导航时的处理方式相同。它已经可以在Chrome浏览器中使用，而且非常棒。这是滚动恢复规范。

由于浏览器开始处理“默认情况”，并且应用具有不同的滚动需求（例如本网站！），因此我们不提供默认滚动管理功能。本指南应帮助您实现任何滚动需求。

### 滚动到顶部

在大多数情况下，您所需要做的只是“滚动到顶部”，因为您有一个较长的内容页面，该页面在导航到该页面时始终保持向下滚动。使用<ScrollToTop>组件可以轻松处理此问题，该组件将在每次导航时向上滚动窗口：

```js
import { useEffect } from "react";
import { useLocation } from "react-router-dom";

export default function ScrollToTop() {
  const { pathname } = useLocation();

  useEffect(() => {
    window.scrollTo(0, 0);
  }, [pathname]);

  return null;
}
```

如果您尚未运行React 16.8，则可以使用React.Component子类执行相同的操作：
```js
import React from "react";
import { withRouter } from "react-router-dom";

class ScrollToTop extends React.Component {
  componentDidUpdate(prevProps) {
    if (
      this.props.location.pathname !== prevProps.location.pathname
    ) {
      window.scrollTo(0, 0);
    }
  }

  render() {
    return null;
  }
}

export default withRouter(ScrollToTop);
```
然后将其呈现在您应用的顶部，但在路由器下方

```js
function App() {
  return (
    <Router>
      <ScrollToTop />
      <App />
    </Router>
  );
}
```
如果您将标签页接口连接到路由器，那么当他们切换标签页时，您可能不想滚动到顶部。相反，关于在您需要的特定位置`<ScrollToTopOnMount>`？

```js
import { useEffect } from "react";

function ScrollToTopOnMount() {
  useEffect(() => {
    window.scrollTo(0, 0);
  }, []);

  return null;
}

// Render this somewhere using:
// <Route path="..." children={<LongContent />} />
function LongContent() {
  return (
    <div>
      <ScrollToTopOnMount />

      <h1>Here is my long content page</h1>
      <p>...</p>
    </div>
  );
}
```

再说一次，如果您还没有运行React 16.8，则可以对React.Component子类做同样的事情：

```js

class ScrollToTopOnMount extends React.Component {
  componentDidMount() {
    window.scrollTo(0, 0);
  }

  render() {
    return null;
  }
}

// Render this somewhere using:
// <Route path="..." children={<LongContent />} />
class LongContent extends React.Component {
  render() {
    return (
      <div>
        <ScrollToTopOnMount />

        <h1>Here is my long content page</h1>
        <p>...</p>
      </div>
    );
  }
}
```

### 通用解决方案

对于通用解决方案（以及哪些浏览器已开始在本机实现），我们谈论的是两件事：

   -    向上滚动导航，这样就不会启动滚动到底部的
   
   -    新屏幕恢复窗口的滚动位置和“后退”和“前进”单击上的溢出元素（但不单击“链接”单击！）
   
在某一时刻，我们希望提供一个通用的API。这就是我们要去的方向：
```js
<Router>
  <ScrollRestoration>
    <div>
      <h1>App</h1>

      <RestoredScroll id="bunny">
        <div style={{ height: "200px", overflow: "auto" }}>
          I will overflow
        </div>
      </RestoredScroll>
    </div>
  </ScrollRestoration>
</Router>
```

-   首先，ScrollRestoration将在导航时向上滚动窗口。
-   其次，它将使用location.key将窗口滚动位置和RestoredScroll组件的滚动位置保存到sessionStorage。
-   然后，在安装ScrollRestoration或RestoredScroll组件时，它们可以从sessionStorage查找其位置。

棘手的部分是为不希望管理窗口滚动的情况定义一个“退出” API。例如，如果您在页面内容内浮动了一些标签导航，则可能不想滚动到顶部（这些标签可能会滚出视线！）。

当我们得知Chrome现在可以为我们管理滚动位置，并意识到不同的应用程序将具有不同的滚动需求时，我们有点迷失了我们需要提供某些东西的信念，尤其是当人们只想滚动到顶部时（您可以直接将其直接添加到您的应用中）。

基于此，我们不再有足够的力气自己完成工作（就像您一样，我们的时间有限！）。但是，我们很乐意为有志于实施通用解决方案的任何人提供帮助。一个可靠的解决方案甚至可以存在于项目中。如果您开始使用它，请与我们联系:)
