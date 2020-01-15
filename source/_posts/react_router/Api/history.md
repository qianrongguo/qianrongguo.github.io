## history
本文档中的术语“历史记录”和“历史记录对象”是指历史记录包，它是React Router仅有的两个主要依赖项之一（除了React本身），并提供了多种不同的实现来管理JavaScript中的会话历史记录。环境。
使用的语法：
   - “browser history”————Dom特殊的实现，在支持HTML5历史记录API的Web浏览器中很有用。
   -    “hash history“————遗留Web浏览器的DOM特定实现。
   -    “memory history”————内存历史记录实现，可用于测试和非DOM环境（例如React Native）
   
历史记录对象通常具有以下属性和方法：
- length ——(number) 历史记录堆栈中的条目数。
- action ——(string)当前 action (PUSH, REPLACE, or POP)
- location ——(object) 当前位置，具有以下属性：
		pathname——（string）URL的路径
		search——（string)URL查询字符串
		hash———（string)URL哈希片段
		state——（object)提供给例如当此位置被压入堆栈时，push（path，state）。仅在浏览器和内存历史记录中可用。
- push(path, [state])——（function）将新条目推入历史记录堆栈
- replace(path, [state]) ——(function)替换历史记录堆栈上的当前条目
- go(n)——(function)将历史记录返回n个。
- goBack()——(function)相当于go(-1)
- goForward()——(function)相当于go(1)
- block(prompt)——(function) 防止导航（请参阅历史记录文档）
### history is mutable
历史对象是可变的。因此建议访问位置使用props`<Route>`的渲染，而不是history.location访问位置。这可以确保您对React的假设在生命周期挂钩中是正确的。例如：

```js
class Comp extends React.Component {
  componentDidUpdate(prevProps) {
    // will be true
    const locationChanged =
      this.props.location !== prevProps.location;

    // INCORRECT, will *always* be false because history is mutable.
    const locationChanged =
      this.props.history.location !== prevProps.history.location;
  }
}

<Route component={Comp} />;
```
根据您所使用的实现方式，可能还会显示其他属性。请参阅历史记录文档以获取更多详细信息。
