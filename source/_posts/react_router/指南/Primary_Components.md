##  Primary Components

React Router中的组件主要分为三类：
      
   -    routers,例如  <BrowserRouter> and <HashRouter>
   -    route匹配，例如  <Route> and <Switch>
   -    导航  例如  <Link>, <NavLink>, and <Redirect>
   
使用的web应运都应从react-router-dom导入。

    import { BrowserRouter, Route, Link } from "react-router-dom";
    Routers

### Routers

   每个React Router应用程序的核心应该是路由器组件。对于Web项目，react-router-dom提供 `<BrowserRouter>`和`<HashRouter>`路由器。两者之间的主要区别在于**它们存储URL和与Web服务器通信的方式**。

   -    `<BrowserRouter>`使用常规URL路径。
        这些通常是外观最好的URL，但是它们要求正确配置服务器。
        具体来说，您的Web服务器需要在所有由React Router客户端管理的URL上提供相同的页面。
        Create React App在开发中即开即用地支持此功能，并附带有关如何配置生产服务器的说明。
   -    `<HashRouter>`将当前位置存储在URL的哈希部分中，因此URL看起来类似于http://example.com/#/your/page。
        由于哈希从不发送到服务器，因此这意味着不需要特殊的服务器配置。
        
   要使用路由器，只需确保将其呈现在元素层次结构的根目录下即可。通常，您会**将顶级<App>元素包装在路由器**中，如下所示：        
   
   ```js
   import React from "react";
   import ReactDOM from "react-dom";
   import { BrowserRouter } from "react-router-dom";
   
   function App() {
     return <h1>Hello React Router</h1>;
   }
   
   ReactDOM.render(
     <BrowserRouter>
       <App />
     </BrowserRouter>,
     document.getElementById("root")
   );
   ```
### Route Matchers

  有两个路由匹配组件：Switch and Route。呈现`<Switch>`时，它将搜索其子`<Route>`元素以查找其路径与当前URL匹配的元素。当找到一个时，它将呈现该`<Route>`并忽略所有其他路由。这意味着您应该将`<Route>`包含更多特定路径（通常较长）的路径放在不那么特定路径之前。
  
  如果没有<Route>匹配，则<Switch>不呈现任何内容（空）

   ```js
    import React from "react";
    import ReactDOM from "react-dom";
    import {
      BrowserRouter as Router,
      Switch,
      Route
    } from "react-router-dom";
    
    function App() {
      return (
        <div>
          <Switch>
            {/* 如果当前URL是/ about，则呈现此路由
                而其余的则被忽略 */}
            <Route path="/about">
              <About />
            </Route>
    
            {/* 请注意这两个路由的顺序。更具体path =" / contact /：id"在path =" / contact"之前，因此查看单个联系人时，路线将呈现 */}
            <Route path="/contact/:id">
              <Contact />
            </Route>
            <Route path="/contact">
              <AllContacts />
            </Route>
            {/* 如果先前的路线都不提供任何东西，
                这条路线充当后备路线。
                重要提示：路径=" /"的路线将*始终*匹配URL，因为所有URL均以/开头。所以那是为什么我们把这一切放在最后
                */}
            <Route path="/">
              <Home />
            </Route>
          </Switch>
        </div>
      );
    }
    
    ReactDOM.render(
      <Router>
        <App />
      </Router>,
      document.getElementById("root")
    );
   ```
   需要注意的重要一件事是`<Route path>`匹配URL的开头，而不是整个开头。因此，`<Route path =“ /”>`将始终与URL匹配。因此，我们通常将此`<Route>`放在`<Switch>`的最后。另一种可能的解决方案是使用确实与整个URL匹配的`<Route exact path="/">`。

   注意：尽管React Router确实支持在`<Switch>`之外渲染`<Route>`元素，但是从5.1版本开始，我们建议您改用useRouteMatch钩子。此外，我们不建议您渲染不带路径的`<Route>`，而是建议您使用钩子来访问所需的任何变量。
### 导航（或路线更改器）
   React Router提供了一个`<Link>`组件来在您的应用程序中创建链接。无论在何处呈现`<Link>`，锚点（`<a>`）都将呈现在HTML文档中。
   ```html
    <Link to="/">Home</Link>
    // <a href="/">Home</a>
```
   `<NavLink>`是`<Link>`的一种特殊类型，当其prop与当前位置匹配时，可以将其自身设置为“active”。
   ```html
    <NavLink to="/react" activeClassName="hurray">
      React
    </NavLink>
    
    // 当 URL 是 /react, 呈现:
    // <a href="/react" className="hurray">React</a>
    
    // 没有设置成active，呈现：
    // <a href="/react">React</a>
```
   任何时候要强制导航，都可以给予`<Redirect>`。当`<Redirect>`呈现时，它将使用其prop进行导航。
   
   ```js
    <Redirect to="/login" />
   ```
