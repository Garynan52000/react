# React 高级应用 -- 高阶组件 Heigher Order Component

**高阶组件** 是 React 中**重用组件的主要实现方式**。但高阶组件本身并不是 React API，它只是一种模式。

具体而言，**高阶组件就是一个函数，且该函数接受一个组件作为参数，并返回一个新的组件。**

```
function Section() {
  return <section>Section</section>
}

function higherOrderComponent(BasicalComponent) {
  return (
    <artical>
      <header>Header</header>
      <hr>
      <BasicalComponent />
      <hr>
      <footer>Footer</footer>
    </artical>
  );
}

const Paga1 = higherOrderComponent(Section);
```

> 注意，高阶组件既**不会修改输入组件，也不会使用继承拷贝它的行为。** 而是，高阶组件 组合（composes） 原始组件，通过用一个容器组件 包裹着（wrapping） 原始组件。高阶组件就是一个没有副作用的纯函数。

简单地说，就是在不污染输入组件的情况下，输出一个功能更丰富的新组件。

```
function Button(props) {
  return <button onClick={props.onClick}>{props.children}</button>
}

function setClickCounter(Component, config = {}) {
  return class extends React.Component {
    constructor(porps) {
      super(porps);
      
      const {name, id, completeTime} = config;

      this.clicked = this.clicked.bind(this);

      this.state = {
        name,
        id,
        completeTime,
        count: 0,
        isFinished: false
      }
    }

    clicked() {
      if (this.state.isFinished) return;
      const {name, id, completeTime} = this.state;
      this.setState(preState => {
        const count = preState.count +1;
        const isFinished = count >= completeTime;
        console.log(`"${name}" 点击了 ${count}/${completeTime} 次 [${isFinished? '已完成':'未完成'}]`);
        return {count, isFinished};
      });
    }
    
    render() {
      const {name, isFinished} = this.state;
      const {children, otherProps} = this.props;
      if (isFinished) return `${name} [已完成]。`;
      return (
        <Component onClick={this.clicked} {...otherProps}>
          {children}
        </Component>
      );
    }
  }
}

const SuperBtnA = setClickCounter(Button, {
  name: 'A点击事件',
  id: 'A',
  completeTime: 10,
});
const SuperBtnB = setClickCounter(Button, {
  name: 'B点击事件',
  id: 'B',
  completeTime: 15,
});
```





















