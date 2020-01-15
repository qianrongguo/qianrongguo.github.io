##	location
location表示该应用程序现在的位置，您希望其运行的位置，甚至是以前的位置。看起来像这样：
```json
{
  key: 'ac3df4', // not with HashHistory!
  pathname: '/somewhere',
  search: '?some=search-string',
  hash: '#howdy',
  state: {
    [userDefined]: true
  }
}
```
路由器提供位置对象：
- Route component as this.props.location
- Route render as ({ location }) => ()
- Route children as ({ location }) => ()
- withRouter as this.props.location
也可以在history.location上找到它，但是您不应使用它，因为它是可变的。您可以在历史记录文档中阅读有关此内容的更多信息。
位置对象永远不会发生变化，因此您可以在生命周期挂钩中使用它来确定什么时候进行导航，这对于数据获取和动画处理非常有用。
```js
componentWillReceiveProps(nextProps) {
  if (nextProps.location !== this.props.location) {
    // navigated!
  }
}
```
您可以提供locations替换各个地方的导航：
- Web Link to
- Native Link to
- Redirect to
- history.push
- history.replace
通常，您只使用字符串，但是如果您需要添加一些“location state”，只要应用返回到该特定位置，该状态就会可用，则可以使用位置对象代替。如果您要基于导航历史而不是仅基于路径（如模式）来分支UI，这将非常有用。

```js
// usually all you need
<Link to="/somewhere"/>

// but you can use a location instead
const location = {
  pathname: '/somewhere',
  state: { fromDashboard: true }
}

<Link to={location}/>
<Redirect to={location}/>
history.push(location)
history.replace(location)
```
最后，您可以将位置传递给以下组件：
- Route
- Switch

这样可以防止他们在路由器状态下使用实际位置。这对于动画和待处理的导航很有用，或者在您想要诱使组件在与真实位置不同的位置进行渲染时，这很有用。
 
