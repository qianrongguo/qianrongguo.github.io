提供围绕应用程序的声明式，可访问的导航。
```jsx harmony
<Link to="/about">About</Link>
```


### to: string
链接位置的字符串表示形式，是通过将位置的路径名，搜索和哈希属性连接起来而创建的。
```jsx harmony
<Link to="/courses?sort=name" />
```

### to: object
可以具有以下任何属性的对象:
- pathname:  表示链接到的路径的字符串。
- search: query 参数的字符串表示形式。
- hash: 网址中的哈希值，例如＃a-hash。
- state:  state位置
```jsx harmony
<Link
  to={{
    pathname: "/courses",
    search: "?sort=name",
    hash: "#the-hash",
    state: { fromDashboard: true }
  }}
/>
```
### to: function
当前位置作为参数传递给该函数的函数，该函数应以字符串或对象的形式返回位置表示形式
```jsx harmony
<Link to={location => ({ ...location, pathname: "/courses" })} />
```
```jsx harmony
<Link to={location => `${location.pathname}?sort=name`} />
```

### replace: bool
如果为true，则单击链接将替换历史记录堆栈中的当前条目，而不是添加新条目。
```jsx harmony
<Link to="/courses" replace />
```

### innerRef: function
从React Router 5.1开始，如果您使用的是React 16，则不需要此道具，因为我们会将ref转发到基础<a>。请改用普通ref替换。

允许访问组件的基础引用。
```jsx harmony
<Link
  to="/"
  innerRef={node => {
    // `node` refers 为了挂载 Dom元素
    // or 卸载时为null
  }}
/>
```
### innerRef: RefObject
从React Router 5.1开始，如果您使用的是React 16，则不需要此道具，因为我们会将ref转发到基础<a>。请改用普通ref替换。

使用React.createRef获取组件的基础引用。

```jsx harmony
let anchorRef = React.createRef()

<Link to="/" innerRef={anchorRef} />
```

### others

您还可以通过props想要在`<a>`上显示的例如标题，id，className等。
