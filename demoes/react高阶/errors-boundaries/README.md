# React 高级应用 -- 错误边界 Error Boundaries

部分 UI 的异常不应该破坏了整个应用。为了解决 React 用户的这一问题，React 16 引入了一种称为 “错误边界” 的新概念。

错误边界的作用在于**捕获其子组件树 JavaScript 异常，记录错误并展示替补的自定义 React 组件**。

## 错误边界组件的实现

让一个类组件变成一个错误边界，只需实现**生命周期方法** `static getDerivedStateFromError()`或者`componentDidCatch()`中的任意一个或两个。

```
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  /**
   * 当捕捉到错误时，使得在更新视图之前有机会更新 state
   * @param error 具体的 JavaScript 异常
   **
  static getDerivedStateFromError(error) {
    // 当捕捉到错误的时候，返回跟新 this.state
    return { hasError: true };
  }

  /**
   * 错误被捕捉完成。
   * @param error 具体的 JavaScript 异常
   * @param info 一个记录着错误信息是从哪个组件里抛出来的对象。里面包含一些关键的信息。
   **
  componentDidCatch(error, info) {
    // 可以在这里记录组件的错误信息
    logErrorToMyService(error, info);
  }

  render() {
    if (this.state.hasError) {
      // 当错误发生时 显示这部分内容
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children; 
  }
}

# 而后你可以像一个普通的组件一样使用：

<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

## 错误边界无法捕获如下错误:

- 事件处理
- 异步代码 （例如 setTimeout 或 requestAnimationFrame 回调函数）
- 服务端渲染
- 错误边界自身抛出来的错误 （而不是其子组件）

## 错误边界的最小单位是组件

这里的错误边界内的内容不会别替换。错误会冒泡至黎其最近的父组件。

```
<header>
  <h2>Header</h2>
  <ErrorBoundary>
  <nav>
    { a[0].asd.dawd }
    <a>Home</a>
    <a>Part1</a>
    <a>Part2</a>
    <a>Part3</a>
  </nav>
  </ErrorBoundary>
  <hr/>
</header>
```

















