##  Code Splitting

网络的一项重要功能是，我们无需让访问者下载整个应用程序即可使用。您可以将代码拆分视为增量下载应用程序。为了实现这中功能我们要使用
 webpack, @babel/plugin-syntax-dynamic-import, and loadable-components.
 
 webpack内置了对动态导入的支持；但是，如果您使用的是Babel（例如，将JSX编译为JavaScript），则需要使用@ babel / plugin-syntax-dynamic-import插件。这是仅语法的插件，这意味着Babel不会进行任何其他转换。该插件仅允许Babel解析动态导入，因此webpack可以将它们捆绑为代码拆分。您的.babelrc应该如下所示：
```.handlebars
{
  "presets": ["@babel/preset-react"],
  "plugins": ["@babel/plugin-syntax-dynamic-import"]
}

```
loadable-components是用于通过动态导入加载组件的库。它自动处理各种边缘情况，并使代码拆分变得简单！这是有关如何使用可加载组件的示例：
```js
import loadable from "@loadable/component";
import Loading from "./Loading.js";

const LoadableComponent = loadable(() => import("./Dashboard.js"), {
  fallback: <Loading />
});

export default class LoadableDashboard extends React.Component {
  render() {
    return <LoadableComponent />;
  }
}
```
这里所有都是它的！只需使用LoadableDashboard（或任何您命名的组件），当您在应用程序中使用它时，它将自动加载并呈现。回退是一个占位符组件，用于在加载实际组件时显示。

### Code Splitting and Server-Side Render

loadable-components包含服务器端渲染的指南。
