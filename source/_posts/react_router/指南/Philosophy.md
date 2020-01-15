##  Philosophy

本指南的目的是说明使用React Router时要具有的思维模型。我们称之为“动态路由”，它与您可能更熟悉的“静态路由”完全不同。

### 静态路由

如果您使用过Rails，Express，Ember，Angular等，则使用了静态路由。在这些框架中，您需要在进行任何渲染之前将路由声明为应用初始化的一部分。React Router pre-v4也是静态的（大部分是静态的）。让我们看一下如何快速配置路由：
```js
// Express Style routing:
app.get("/", handleIndex);
app.get("/invoices", handleInvoices);
app.get("/invoices/:id", handleInvoice);
app.get("/invoices/:id/edit", handleInvoiceEdit);

app.listen();
```

请注意在应用监听之前如何声明路由。我们使用的客户端路由器相似。在Angular中，您先声明路线，然后在渲染之前将其导入顶级AppModule：
```js
// Angular Style routing:
const appRoutes: Routes = [
  {
    path: "crisis-center",
    component: CrisisListComponent
  },
  {
    path: "hero/:id",
    component: HeroDetailComponent
  },
  {
    path: "heroes",
    component: HeroListComponent,
    data: { title: "Heroes List" }
  },
  {
    path: "",
    redirectTo: "/heroes",
    pathMatch: "full"
  },
  {
    path: "**",
    component: PageNotFoundComponent
  }
];

@NgModule({
  imports: [RouterModule.forRoot(appRoutes)]
})
export class AppModule {}
```
Ember具有常规的route.js文件，该版本会为您读取并导入到应用程序中。同样，这是在您的应用渲染之前发生的。

```js
// Ember Style Router:
Router.map(function() {
  this.route("about");
  this.route("contact");
  this.route("rentals", function() {
    this.route("show", { path: "/:rental_id" });
  });
});

export default Router;
```

尽管API不同，但它们都共享“静态路由”模型。React Router也跟进了直到v4。

为了成功使用React Router，您需要忘记所有这些！：O

### Backstory
坦率地说，我们对v2采取React Router的方向感到非常沮丧。我们（Michael和Ryan）感到受API的限制，认识到我们正在重新实现React的各个部分（生命周期等），而这与React为构建UI提供的思维模型不符。

我们正要经过车间讨论要怎么做的研讨会前的酒店走廊。我们互相问：“如果使用我们在讲习班中教授的模式建造路由器，那会是什么样？”

仅仅几个小时的开发时间，我们就获得了概念证明，我们知道这是我们想要路由的未来。我们最终得到的API并不是React的“外部”，它是由React的其余部分组成或自然地融入其中的。我们认为您会喜欢的。


### 动态 Routing

当说动态路由时，是指在您的应用渲染时发生的路由，而不是在运行的应用之外的配置或约定中进行。这意味着几乎所有内容都是React Router中的一个组件。这是对该API的60秒回顾，以了解其工作原理：

首先，为您要定位的环境获取一个Router组件，并将其呈现在应用程序的顶部。

```js
// react-native
import { NativeRouter } from "react-router-native";

// react-dom (what we'll use here)
import { BrowserRouter } from "react-router-dom";

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  el
);
```
接下来，获取链接组件以链接到新位置：
```js
const App = () => (
  <div>
    <nav>
      <Link to="/dashboard">Dashboard</Link>
    </nav>
  </div>
);
```

最后，渲染一个Route以在用户访问/ dashboard时显示一些UI。
```js
const App = () => (
  <div>
    <nav>
      <Link to="/dashboard">Dashboard</Link>
    </nav>
    <div>
      <Route path="/dashboard" component={Dashboard} />
    </div>
  </div>
);
```
路线将渲染<Dashboard {... props} />，其中道具是路由器特定的东西，看起来像{匹配，位置，历史}。如果用户不在/ dashboard上，则Route将呈现null。差不多就够了。

### 嵌套路线

许多路由器都具有“嵌套路由”的概念。如果您使用了v4之前的React Router版本，那么您也会知道它也是如此！当您从静态路由配置转移到动态渲染的路由时，如何“嵌套路由”？好吧，如何嵌套div？
```js
const App = () => (
  <BrowserRouter>
    {/* here's a div */}
    <div>
      {/* here's a Route */}
      <Route path="/tacos" component={Tacos} />
    </div>
  </BrowserRouter>
);

// 当网址与`/ tacos`相匹配时，此组件呈现
const Tacos = ({ match }) => (
  // here's a nested div
  <div>
    {/* here's a 嵌套 Route,
        match.url 帮助我们建立相对的路径 */}
    <Route path={match.url + "/carnitas"} component={Carnitas} />
  </div>
);
```
看看路由器如何没有“嵌套” API？就像div一样，Route只是一个组件。因此，要嵌套一个Route或一个div，您只需…做就可以了。

### 响应路线

考虑用户导航到'    /invoices   '。您的应用程序适应不同的屏幕尺寸，它们的视口狭窄，因此您只向他们显示发票清单和发票仪表板的链接。他们可以从那里更深入地导航。
```text
Small Screen
url: /invoices

+----------------------+
|                      |
|      Dashboard       |
|                      |
+----------------------+
|                      |
|      Invoice 01      |
|                      |
+----------------------+
|                      |
|      Invoice 02      |
|                      |
+----------------------+
|                      |
|      Invoice 03      |
|                      |
+----------------------+
|                      |
|      Invoice 04      |
|                      |
+----------------------+
```
在较大的屏幕上，我们想显示一个主从视图，其中导航在左侧，仪表板或特定发票在右侧。
```text
Large Screen
url: /invoices/dashboard

+----------------------+---------------------------+
|                      |                           |
|      Dashboard       |                           |
|                      |   Unpaid:             5   |
+----------------------+                           |
|                      |   Balance:   $53,543.00   |
|      Invoice 01      |                           |
|                      |   Past Due:           2   |
+----------------------+                           |
|                      |                           |
|      Invoice 02      |                           |
|                      |   +-------------------+   |
+----------------------+   |                   |   |
|                      |   |  +    +     +     |   |
|      Invoice 03      |   |  | +  |     |     |   |
|                      |   |  | |  |  +  |  +  |   |
+----------------------+   |  | |  |  |  |  |  |   |
|                      |   +--+-+--+--+--+--+--+   |
|      Invoice 04      |                           |
|                      |                           |
+----------------------+---------------------------+
```

现在暂停一分钟，并考虑两种屏幕尺寸的  /invoices 网址。它甚至是大屏幕的有效路线吗？我们应该在右边放什么？
```text
Large Screen
url: /invoices
+----------------------+---------------------------+
|                      |                           |
|      Dashboard       |                           |
|                      |                           |
+----------------------+                           |
|                      |                           |
|      Invoice 01      |                           |
|                      |                           |
+----------------------+                           |
|                      |                           |
|      Invoice 02      |             ???           |
|                      |                           |
+----------------------+                           |
|                      |                           |
|      Invoice 03      |                           |
|                      |                           |
+----------------------+                           |
|                      |                           |
|      Invoice 04      |                           |
|                      |                           |
+----------------------+---------------------------+
```

在大屏幕上，/invoices 不是有效的路径，但在小屏幕上则是！为了使事情变得更有趣，请考虑使用大型手机的人。他们可能会纵向查看/发票，然后将手机旋转至横向。突然，我们有足够的空间来显示主从界面，因此您应该立即进行重定向！


React Router以前版本的静态路由并没有真正解决这个问题的方法。但是，当路由是动态的时，您可以声明性地组合此功能。如果您开始考虑将路由选择为UI，而不是静态配置，那么您的直觉将引导您进入以下代码

```js
const App = () => (
  <AppLayout>
    <Route path="/invoices" component={Invoices} />
  </AppLayout>
);

const Invoices = () => (
  <Layout>
    {/* 总是显示导航 */}
    <InvoicesNav />

    <Media query={PRETTY_SMALL}>
      {screenIsSmall =>
        screenIsSmall ? (
          // 小屏幕没有重定向
          <Switch>
            <Route
              exact
              path="/invoices/dashboard"
              component={Dashboard}
            />
            <Route path="/invoices/:id" component={Invoice} />
          </Switch>
        ) : (
          // 大屏幕
          <Switch>
            <Route
              exact
              path="/invoices/dashboard"
              component={Dashboard}
            />
            <Route path="/invoices/:id" component={Invoice} />
            <Redirect from="/invoices" to="/invoices/dashboard" />
          </Switch>
        )
      }
    </Media>
  </Layout>
);
```

当用户将手机从纵向旋转到横向时，此代码将自动将其重定向到仪表板。有效路线集会根据用户手中移动设备的动态性质而变化。


这只是一个例子。我们可以讨论许多其他内容，但我们将总结以下建议：为了使您的直觉与React Router的直觉相符，请考虑组件而不是静态路由。考虑一下如何使用React的声明式可组合性解决问题，因为几乎每个“ React Router问题”都可能是“ React问题”。

