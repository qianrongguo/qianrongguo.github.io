这使您可以使用`<Route>`使用的相同匹配代码，但不在正常渲染周期之内，例如在服务器上渲染之前收集数据依赖项。
```jsx harmony
import { matchPath } from "react-router";

const match = matchPath("/users/123", {
  path: "/users/:id",
  exact: true,
  strict: false
});
```
### pathname
第一个参数是您要匹配的路径名。如果您在带有Node.js的服务器上使用它，则为req.path

### props
第二个参数是要匹配的道具，它们与Route接受的匹配道具相同。它也可以是字符串或字符串数​​组，作为{path}的快捷方式。
```jsx harmony
{
  path, // like /users/:id; either a single string or an array of strings
  strict, // optional, defaults to false
  exact  // optional, defaults to false
}
```

### returns
当提供的路径名与路径属性匹配时，它将返回一个对象。
```jsx harmony
matchPath("/users/2", {
  path: "/users/:id",
  exact: true,
  strict: true
});
//  {
//    isExact: true
//    params: {
//        id: "2"
//    }
//    path: "/users/:id"
//    url: "/users/2"
//  }
```
如果提供的路径名与路径属性不匹配，则返回null。
```jsx harmony
matchPath("/users", {
  path: "/users/:id",
  exact: true,
  strict: true
});

//  null
```

