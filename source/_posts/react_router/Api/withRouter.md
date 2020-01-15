您可以通过withRouter高阶组件访问历史对象的属性和最接近的`<Route>`匹配项。每当呈现时，withRouter都会将更新的匹配，位置和历史道具传递给包装的组件。
```jsx harmony
import React from "react";
import PropTypes from "prop-types";
import { withRouter } from "react-router";

// A simple component that shows the pathname of the current location
class ShowTheLocation extends React.Component {
  static propTypes = {
    match: PropTypes.object.isRequired,
    location: PropTypes.object.isRequired,
    history: PropTypes.object.isRequired
  };

  render() {
    const { match, location, history } = this.props;

    return <div>You are now at {location.pathname}</div>;
  }
}

// Create a new component that is "connected" (to borrow redux
// terminology) to the router.
const ShowTheLocationWithRouter = withRouter(ShowTheLocation);
```

重要的提示

withRouter不像React Redux的connect那样订阅位置更改以进行状态更改。而是在位置更改后从`<Router>`组件传播出去后重新渲染。这意味着withRouter不会在路由转换时重新呈现，除非其父组件重新呈现。

静态方法和属性

包装组件的所有非特定于反应的静态方法和属性将自动复制到“connected”组件。

### Component.WrappedComponent
包装的组件在返回的组件上作为静态属性WrappedComponent公开，它可以用于隔离测试组件等。
```jsx harmony
// MyComponent.js
export default withRouter(MyComponent)

// MyComponent.test.js
import MyComponent from './MyComponent'
render(<MyComponent.WrappedComponent location={{...}} ... />)
```

### wrappedComponentRef: func

该函数将作为ref prop传递给包装的组件。

```jsx harmony
class Container extends React.Component {
  componentDidMount() {
    this.component.doSomething();
  }

  render() {
    return (
      <MyComponent wrappedComponentRef={c => (this.component = c)} />
    );
  }
}
```
