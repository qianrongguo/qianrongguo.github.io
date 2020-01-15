##  Redux整合

Redux是React生态系统的重要组成部分。对于想要同时使用React Router和Redux的人，我们希望使其无缝集成。

### 阻止的更新
通常，React Router和Redux可以很好地协同工作。不过，有时候，应用程序的组件可能会在位置更改时（子路线或活动的导航链接不更新）不更新。


在以下情况下会发生这种情况：
    1.该组件通过connect（）（Comp）连接到redux。
    2.该组件不是“路由组件”，这意味着它的呈现方式不是这样：`<Route component = {SomeConnectedThing} />`


问题在于Redux实现了shouldComponentUpdate，如果没有从路由器接收道具，则没有任何迹象表明发生了任何变化。这很容易解决。查找连接组件的位置，然后将其与Router包装在一起。
```js
// before
export default connect(mapStateToProps)(Something)

// after
import { withRouter } from 'react-router-dom'
export default withRouter(connect(mapStateToProps)(Something))
```

### 深度整合

有些人想：
    1.与商店同步并从商店访问路由数据。
    2.能够通过调度动作进行导航。
    3.在Redux devtools中支持对路径更改进行时间旅行调试。

所有这些都需要更深入的集成。

我们的建议是不要将路线完全保留在Redux商店中。推理:

   1.路由数据已经成为大多数关心它的组件的支持。无论是来自商店还是路由器，您组件的代码都基本相同。
   
   2.在大多数情况下，您可以使用链接，导航链接和重定向来执行导航操作。有时，在某些最初由操作启动的异步任务之后，您可能还需要以编程方式导航。例如，您可以在用户提交登录表单时调度操作。然后，您的重击，传奇或其他异步处理程序会对凭据进行身份验证，如果成功，则需要以某种方式导航到新页面。此处的解决方案只是将历史对象（提供给所有路由组件）包括在操作的有效负载中，并且异步处理程序可以在适当的时候使用此对象进行导航。
   
   3.路线更改对于时间旅行调试不太重要。唯一明显的情况是调试路由器/商店同步中的问题，如果根本不同步它们，则该问题将消失。
    

但是，如果您强烈希望与商店同步路由，则可以尝试使用Connected React Router，这是React Router v4和Redux的第三方绑定。
