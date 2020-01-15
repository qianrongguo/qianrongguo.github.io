一个`<Router>`，用于将“ URL”的历史记录保留在内存中（不读取或写入地址栏）。在测试和非浏览器环境（如React Native）中很有用。

```jsx harmony
<MemoryRouter
  initialEntries={optionalArray}
  initialIndex={optionalNumber}
  getUserConfirmation={optionalFunc}
  keyLength={optionalNumber}
>
  <App />
</MemoryRouter>
```

### initialEntries: array
历史记录堆栈中的位置数组。这些可能是带有{路径名，搜索，哈希，状态}或简单字符串URL的成熟位置对象。
```jsx harmony
<MemoryRouter
  initialEntries={["/one", "/two", { pathname: "/three" }]}
  initialIndex={1}
>
  <App />
</MemoryRouter>

```

### initialIndex: number
初始位置在initialEntries数组中的索引。


### getUserConfirmation: func
用于确认导航的功能。直接将`<MemoryRouter>`与`<Prompt>`一起使用时，必须使用此选项。


### keyLength: number
location.key的长度。默认为6。
```jsx harmony
<MemoryRouter keyLength={12} />
```

### children: node
要渲染的子元素。

注意：在React <16上，您必须使用单个子元素，因为render方法不能返回多个元素。如果需要多个元素，则可以尝试将它们包装在额外的`<div>`中。
