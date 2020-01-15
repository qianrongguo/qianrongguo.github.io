```jsx harmony
import React, { Component } from "react";
import { StaticRouter as Router, Route } from "react-router-dom";

// 本示例在StaticRouter中渲染路由并填充其路由staticContext，然后将其打印出来。在现实世界中，您会使用StaticRouter进行服务器端渲染：
//
// import express from "express";
// import ReactDOMServer from "react-dom/server";
//
// const app = express()
//
// app.get('*', (req, res) => {
//   let staticContext = {}
//
//   let html = ReactDOMServer.renderToString(
//     <StaticRouter location={req.url} context={staticContext}>
//       <App /> (includes the RouteStatus component below e.g. for 404 errors)
//     </StaticRouter>
//   );
//
//   res.status(staticContext.statusCode || 200).send(html);
// });
//
// app.listen(process.env.PORT || 3000);

function RouteStatus(props) {
  return (
    <Route
      render={({ staticContext }) => {
        // 我们必须检查staticContext是否存在//，因为如果通过BrowserRouter呈现，它将是未定义的
        if (staticContext) {
          staticContext.statusCode = props.statusCode;
        }

        return <div>{props.children}</div>;
      }}
    />
  );
}

function PrintContext(props) {
  return <p>Static context: {JSON.stringify(props.staticContext)}</p>;
}

export default class StaticRouterExample extends Component {
  // 这是我们传递给StaticRouter的上下文对象。可以通过路由进行修改以提供其他信息,用于服务器端渲染。
  staticContext = {};

  render() {
    return (
      <Router location="/foo" context={this.staticContext}>
        <div>
          <RouteStatus statusCode={404}>
            <p>Route with statusCode 404</p>
            <PrintContext staticContext={this.staticContext} />
          </RouteStatus>
        </div>
      </Router>
    );
  }
}
```
