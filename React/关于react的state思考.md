**为什么 react 的 state 要分成一个 getting ，一个 setting ？**



从查阅的资料来看，react 中的 state 并未具体要求必须分成 getting 和 setting，我更多认为这可是对 React 中的 state 使用模式的一种理解或者一种形如约定俗成的概念。



从官方介绍中，react 的 useState 是用来在函数组件中管理状态的 Hook。它返回一个数组，其中第一个元素是当前的状态值，第二个元素是可以用来更新状态的函数。



在使用 useState 创建状态时，我们通常会将其分解为两个变量，例如：



```
const [count, setCount] = useState<number>(0);
```



在示范代码中 count 是当前的状态值，而 setCount 是用于更新状态的函数。这种约定是为了提高代码的可读性和易用性。



可以看出 get 和 set 这两个名称并不是 React 或者 useState 的特定规定，而是一种命名习惯。get 通常用于表示获取状态值的操作，而 set 表示设置（更新）状态值的操作。



当你调用 setCount ( newValue )  时，React 会重新渲染组件，并将 newValue 设置为 count 的新值。



综上所述，get 和 set 只是作为 useState 的约定命名，用于获取和设置状态值，以提高代码的可读性。实际上，你可以选择其他的变量名，只要它们符合语法规则即可。



同时，为了更好地控制组件的状态管理和数据流动，开发者会选择将 state 分为 getter 和 setter。这类似于面向对象编程中的属性（ property ）的概念。



通过将 state 分成 getter 和 setter，我们可以实现更细粒度的控制和封装。getter 用于获取 state 的值，setter 用于更新 state 的值。这样做的目的是为了遵循封装原则，限制外部对 state 的直接访问和修改，提高代码的可维护性和可测试性。



同时对于 React 而言，将状态的获取和设置分开可以带来一些性能优化的好处。



当状态被获取时（即通过获取函数进行读取），React 可以追踪组件对该状态的依赖关系。这意味着 React 知道哪些组件依赖于特定的状态值。如果状态没有被修改，React 可以跳过那些依赖于该状态的组件的重新渲染，从而减少不必要的计算和 DOM 操作。



另一方面，当状态被设置时（即通过设置函数进行更新），React 会触发组件的重新渲染，并将新的状态值传递给相应的组件。React 可以确保界面与最新的状态值保持同步。



通过区分获取和设置函数，React 可以更好地理解和优化组件的更新行为。React 使用一种称为 "Virtual DOM" 的机制来比较前后两次渲染的差异，并仅更新需要变化的部分，而不是整个页面。这种优化技术称为 "Diffing Algorithm"（差异算法）。



此外，将状态的获取和设置分开还有助于在某些情况下使用 useMemo 和 useCallback 这样的 Hook 进行性能优化。这些 Hook 可以缓存计算结果或回调函数，只有在依赖项发生更改时才重新计算或创建新的回调函数，从而减少不必要的计算开销。



但性能优化并非绝对，在实际情况中的效果取决于具体的代码和使用方式。因此，在进行性能优化时，应该结合具体场景进行评估和测试，以确保获得预期的结果（具体可以指路滥用 useMmeo 导致的案例：内存开销，缓存失效、延迟加载、性能并发问题）。

下例我做了一个简单的组件，它根据用户点击按钮的次数，计算并显示斐波那契数列的第 n 项：



```
import React, { useState, useMemo } from "react";

export const Fibonacci = () => {
  const [count, setCount] = useState(1);

  const calculateFibonacci = (num) => {
    if (num <= 2) return 1;

    return calculateFibonacci(num - 1) + calculateFibonacci(num - 2);
  };

  const handleClick = () => {
    setCount(count + 1);
  };

  const fibNumber = calculateFibonacci(count);

  return (
    <div>
      <p>
        斐波那契数列的第{count}项是：{fibNumber}
      </p>
      <button onClick={handleClick}>增加</button>
    </div>
  );
};

```



在上述代码中，每次用户点击按钮时，都会调用 calculateFibonacci 函数来计算斐波那契数列的第 n 项。由于斐波那契数列的计算量很大，随着 n 的增加，计算时间会显著增加。因此我选择采用 useMemo 来缓存计算结果，只有当 count 发生变化时才重新计算斐波那契数列的值



```
export const useFibonacci = () => {
  const [count, setCount] = useState(1);

  const calculateFibonacci = useMemo(() => {
    const fibonacci = (num) => {
      if (num <= 2) return 1;

      return fibonacci(num - 1) + fibonacci(num - 2);
    };

    return fibonacci;
  }, []);

  const handleClick = () => {
    setCount(count + 1);
  };

  const fibNumber = calculateFibonacci(count);

  return (
    <div>
      <p>
        斐波那契数列的第{count}项是：{fibNumber}
      </p>
      <button onClick={handleClick}>增加</button>
    </div>
  );
};
```



修改后的代码中，我使用 useMemo 将 useFibonacci 缓存起来。只有在组件挂载时才会创建新的函数。每次渲染时，如若count没有变化，就会直接使用缓存的函数进行计算，避免不必要的重复计算开销。

例子中使用了性能优化的技巧，但是由于斐波那契数列的计算量很大，当 count 较大时，仍然会因为计算斐波那契数列而导致性能下降。因为每次更新 count 后，仍然需要重新计算斐波那契数列的值（ Tips:  正向使用 useMemo，仍会导致性能下降。从某种意义假使认为：正向非正向使用所带来的效果并非是绝对的）。



在特定的情况下，尽管使用了useMemo 进行了性能优化，但在 count 较大的情况下，仍然无法避免性能问题。表明性能还是取决于具体的代码和使用方式，需要根据具体场景进行评估和测试，以期获得性能改进和提升。



总结：



使用 useState 时将状态值和更新函数分别赋值给不同的变量（通常是使用解构赋值方式），有以下好处：



1. 提高可读性： 通过使用具有描述性的变量名，可以更清晰地表达代码的意图。在阅读代码时，很容易理解哪个变量用于获取状态值，哪个变量用于更新状态值。

2. 简化语法： 将状态值和更新函数分别赋值给不同的变量，可以减少重复输入的代码量。当你需要获取或更新状态值时，只需要使用相应的变量名，而无需每次都写完整的 useState 调用。

3. 避免命名冲突： 如果将状态值和更新函数都存储在同一个变量中，可能会导致命名冲突。通过将它们分配给不同的变量，可以避免潜在的命名冲突问题，使代码更加健壮。

4. 增加灵活性： 将状态值和更新函数分开存储，使得可以轻松地在组件中引入额外的逻辑和操作。例如，可以对状态值进行计算、过滤或映射，而无需修改更新函数的实现。