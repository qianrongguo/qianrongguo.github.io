`<Link>`的特殊版本，当它与当前URL匹配时，它将为rendered的元素添加样式属性。
```jsx harmony
<NavLink to="/about">About</NavLink>
```

### activeClassName: string
元素处于active状态时提供的类。默认给定的类是active的。这将与className prop一起加入。
```jsx harmony
<NavLink to="/faq" activeClassName="selected">
  FAQs
</NavLink>
```

### activeStyle: object
元素处于活动状态时应用于元素的样式。
```jsx harmony
<NavLink
  to="/faq"
  activeStyle={{
    fontWeight: "bold",
    color: "red"
  }}
>
  FAQs
</NavLink>
```
### exact: bool
如果为true，则仅在位置完全匹配时才应用活动的类/样式。
```jsx harmony
<NavLink exact to="/profile">
  Profile
</NavLink>
```
### strict: bool
如果为true，则在确定位置是否与当前URL匹配时，将考虑位置路径名上的斜杠。有关更多信息，请参见`<Route strict>`文档。
```jsx harmony
<NavLink strict to="/events/">
  Events
</NavLink>
```

### isActive: func

一种添加额外逻辑以确定链接是否处于活动状态的功能。如果您要做的事情不仅仅是验证链接的路径名是否与当前URL的路径名匹配，则应使用此选项。
```jsx harmony
<NavLink
  to="/events/123"
  isActive={(match, location) => {
    if (!match) {
      return false;
    }

    // only consider an event active if its event id is an odd number
    const eventID = parseInt(match.params.eventID);
    return !isNaN(eventID) && eventID % 2 === 1;
  }}
>
  Event 123
</NavLink>
```

### location: object
isActive比较当前历史记录位置（通常是当前浏览器URL）。要与其他位置进行比较，可以传递一个位置。

### aria-current: string

活动链接上使用的aria-current属性的值。可用值为：
- "page": 用于指示一组分页链接中的链接。
- "step": 用于指示基于步骤的过程的步骤指示器中的链接
- "location": 用于指示视觉上突出显示的图像作为流程图的当前组成部分。
- "date": 用于指示日历中的当前日期。
- "time": 用于指示时间表中的当前时间。
- "true": 用于指示NavLink是否处于活动状态
默认为“page”。

基于WAI-ARIA 1.1规范



