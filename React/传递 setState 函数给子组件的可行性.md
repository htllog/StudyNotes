# 传递 setState 函数给子组件的可行性及推荐做法

> React 中存在两种数据传递



React 中存在两种类型的数据传递：props（属性）和 state（状态）:

1. Props 传值：父组件可以通过 props 将数据传递给子组件，这种传值是单向的，即父组件向子组件传递数据，子组件不能直接修改父组件传来的 props。当父组件的 props 发生变化时，react 会重新自动渲染子组件。
2. state 传值：组件的状态时是组件内部的管理数据，通过使用 `setState` 的方法，可以更新组件的状态，并触发组件的重新渲染。该组件传值方式适用于组件内部需要管理和改变的数据。



使用：

如果需要组件之间进行数据传递，但无需在子组件内部改变数据时，使用 props 传值就够了，不需要使用 `setState` 方法。相反，如果需要在组件内部管理和改变数据，可以使用 `setState` 改变状态



> 单向数据流思想



recat 中，向下传递 `setState` 来重新定义状态不是一种推荐的做法，因为 react 鼓励使用单向数据流思想



单向数据流指数据在应用传递的方向是**单向**的，从父组件流向子组件，单向数据流思想有助于提高代码的可维护性、可测试性和可预测性。



1. 父组件是数据的所有者和管理者：父组件负责管理和维护状态数据，是数据唯一来源
2. 父组件通过props传递数据给子组件：父组件将数据通过props传递给子组件，子组件通过props获取数据并进行展示或其他操作
3. 子组件不能直接修改父组件传递的 props ：子组件是被动的，它们不能直接修改props中的数据。props在子组件中是只读的
4. 子组件通过回调函数将数据传递回父组件：如果子组件需要更新数据，它会通过回调函数将新的数据传递回父组件。父组件接收到子组件传递的数据后，可以更新状态并重新渲染。



React 单向数据流并非遵循绝对的限制，特定情况下使用状态管理库（ redux ）或上下文（ context ）来共享和管理状态，但是仍让遵循数据的单向流动原则。



> React 使用回调函数向上更新状态的正确做法



```tsx
import React, { useState } from 'react';
import ChildComponent from './ChildComponent';

export const ParentComponent = () => {
  const [count, setCount] = useState<number>(0);

  const handleCountUpdate = (newCount: number) => {
    setCount(newCount);
  };

  return (
    <>
      <ChildComponent count={count} onCountUpdate={handleCountUpdate} />
    </>
  );
};
```



```tsx
interface ChildComponentProps {
  count: number;
  onCountUpdate: (newCount: number) => void;
}

export const ChildComponent = (props) => {
  const handleClick = () => {
    const newCount = props.count + 1;
    
    props.onCountUpdate(newCount);
  };

  return (
    <>
      <button onClick={handleClick}>Increment Count</button>
    </>
  );
};
```



> 子组件调用 `props.setState` 问题

1. 过度耦合：子组件操作父组件状态，使得子组件和父组件产生紧密的耦合，使得代码难以维护和扩展，降低了组件的可复用性
2. 逻辑分离：将状态更新的逻辑放在父组件中，使得父组件成为状态的管理者。这样有助于代码的组织和维护，使得父组件的职责更加清晰明确
3. 数据流可追踪：通过回调函数将数据传递给父组件，可以清晰地追踪数据的来源和流动路径。可以更好地理解组件之间的关系，并更容易调试和排查问题



**Tips：**

组件之间**耦合过度**，即意味之间的关联性和相互依赖变得紧密，将会导致难以单独修改、测试、维护和重用



问题：

1. 可维护性差：一个组件修改的同时需要修改其他相关组件，代码变得复杂且难维护，会因：修改一个组件引发其他想不到的副作用，导致其他组件出现错误
2. 可测试性差：过度耦合的组件难以进行进行单独的单元测试 -- 因为和其余组件联系过度紧密，测试单个组件可能意味着要测试多个组件，增加测试复杂性和依赖关系
3. 可复用性低：过度耦合的组件在其他上下文中难以重用，因为它们与特定的父组件或上下文紧密关联。复用组件时需要额外处理和解耦，增加了开发的工作量



> 总结



传递 `setState`  函数给子组件是可行的，但是并不是 react 官方推荐的做法：

1. 可行性
   * 在父组件中定义 `setState` 的函数，并通过 props 传递给子组件，子组件可以直接调用 `props.setState` 来更新父组件状态
   * 从功能上而言是有效的，可以实现子组件修改父组件的状态
2. react 官方推荐做法
   * react 官方更倾向于使用回调函数的方式来实现子组件向父组件传递数据和更新状态
   * 使用回掉函数的方式可以遵循 react 单向数据流原则，并使组件之间的数据传递更加清晰和可维护
   * 讲状态更新的逻辑放在父组件中，使父组件成为状态管理者，有助于代码的组织和维护，使得父组件的职责更加明确
   * 通过回调函数将数据传递给父组件，可以更清晰地追踪数据的来源和流动路径，更易于调试和排查问题