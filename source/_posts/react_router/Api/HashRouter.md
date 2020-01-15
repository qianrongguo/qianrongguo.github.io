`<Router>`使用URL的哈希部分（即window.location.hash）使UI与URL保持同步。

**重要说明**：哈希历史记录不支持location.key或location.state。在以前的版本中，我们尝试对行为进行匀称处理，但存在一些无法解决的极端情况。任何需要此行为的代码或插件都将无法使用。由于此技术仅旨在支持旧版浏览器，因此建议您将服务器配置为与`<BrowserHistory>`一起使用。
```jsx harmony
<HashRouter
  basename={optionalString}
  getUserConfirmation={optionalFunc}
  hashType={optionalString}
>
  <App />
</HashRouter>
```

### basename: string
所有位置的基本URL。格式正确的基本名称应以斜杠开头，但不能以斜杠结尾。
```jsx harmony
<HashRouter basename="/calendar"/>
<Link to="/today"/> // renders <a href="#/calendar/today">
```

### getUserConfirmation: func
用于确认导航的功能。默认使用window.confirm。
```jsx harmony
<HashRouter
  getUserConfirmation={(message, callback) => {
    // this is the default behavior
    const allowTransition = window.confirm(message);
    callback(allowTransition);
  }}
/>
```

### hashType: string
用于window.location.hash的编码类型。可用值为：
- "slash"：创建＃/ 和 ＃/sunshine/lollipops之类的哈希
- "slash"：创建hash # and #sunshine/lollipops
- "hashbang"：创建“可抓取的ajax”（Google弃用）哈希l#!/ and #!/sunshine/lollipops
默认为“斜线”。
