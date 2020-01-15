匹配对象包含有关`<Route path>`如何与URL匹配的信息。匹配对象包含以下属性：
- params —— (object) Key/value对从 路径的动态段对应的URL解析
- isExact —— (boolean)true 如果整个URL都匹配（没有结尾字符）
- path —— (string) 用于匹配的路径模式。用于构建嵌套的`<Route>`
- url —— (string) URL的匹配部分。对于构建嵌套的`<Link>`有用

您将可以在各个地方匹配对象：

- Route component as this.props.match
- Route render as ({ match }) => ()
- Route children as ({ match }) => ()
- withRouter as this.props.match
- matchPath as the return value

如果路线没有路径，因此始终匹配，则将获得最接近的父项匹配项。路由器也是如此。
### null matches

即使子路径的路径与当前位置不匹配，使用子项道具的`<Route>`也会调用其子函数。在这种情况下，匹配将为空。能够在匹配时呈现`<Route>`的内容可能会很有用，但是这种情况会带来一些挑战。
“解析” URL的默认方法是将match.url字符串连接到“相对”路径。
```jsx harmony
let path = `${match.url}/relative-path`;
```
如果在匹配为null时尝试执行此操作，则最终将出现TypeError。这意味着在使用子道具时尝试在`<Route>`内部加入“相对”路径是不安全的。

当在生成空匹配对象的`<Route>`中使用无路径`<Route>`时，会发生类似但更微妙的情况。
```jsx harmony
// location.pathname = '/matches'
<Route path="/does-not-match"
  children={({ match }) => (
    // match === null
    <Route
      render={({ match: pathlessMatch }) => (
        // pathlessMatch === ???
      )}
    />
  )}
/>
```
无路径`<Route>`从其父级继承其match对象。如果其父匹配项为null，则其匹配项也将为null。这意味着a）任何子级路由/链接都必须是绝对的，因为没有父级可以解析，并且b）父级匹配可以为null的无路径路由将需要使用子级prop进行渲染。
