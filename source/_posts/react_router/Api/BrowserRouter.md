使用HTML5历史记录API（pushState，replaceState和popstate事件）的`<Router>`使UI与URL保持同步。
```jsx harmony
<BrowserRouter
  basename={optionalString}
  forceRefresh={optionalBool}
  getUserConfirmation={optionalFunc}
  keyLength={optionalNumber}
>
  <App />
</BrowserRouter>
```

### basename: string
所有位置的基本URL。如果您的应用是通过服务器上的子目录提供的，则需要将其设置为子目录。格式正确的基本名称应以斜杠开头，但不能以斜杠结尾。
```jsx harmony
<BrowserRouter basename="/calendar" />
<Link to="/today"/> // renders <a href="/calendar/today">
```
### getUserConfirmation: func
用于确认导航的功能。默认使用window.confirm。
```jsx harmony
<BrowserRouter
  getUserConfirmation={(message, callback) => {
    // this is the default behavior
    const allowTransition = window.confirm(message);
    callback(allowTransition);
  }}
/>
```

###  forceRefresh: bool
如果为true，则路由器将在页面导航中使用整页刷新。您可能希望使用它来模仿传统的服务器渲染应用程序在页面导航之间刷新整个页面的方式。
```jsx harmony
<BrowserRouter forceRefresh={true} />

```
 
### keyLength: number
location.key的长度。默认为6。
```jsx harmony
<BrowserRouter keyLength={12} />
```

### children: node
要渲染的子元素。

注意：在React <16上，您必须使用单个子元素，因为render方法不能返回多个元素。如果需要多个元素，则可以尝试将它们包装在额外的`<div>`中。
