> useMemo



useMemo 是用于渲染期间记忆计算结果的 hook ，能够缓存某个函数返回的值，在依赖项改变时重新计算，因此使用 useMemo 可以优化性能。



> useMemo 滥用的问题



在使用 uesMemo 的时候，当面对存储对象或数组等大量数据，useMemo 缓存的值不会被自动垃圾回收，滥用 useMemo 会增加性能开销。

 

> 代码示例



```tsx
import { useState } from "react";

export const useMemoExample = () => {
  const [color, setColor] = useState<string>("white");

  return (
    <>
      <input
        value={color}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
          setColor(e.target.value)
        }
      />
      <p style={{ color }}>你好，世界！</p>
    	<ExpensiveTree />
    </>
  );
};

export const ExpensiveTree = () => {
  const now = performance.now();

  while (performance.now() - now < 100) {
    // 在代码中进行人为延迟，在 100 ms 内不做任何操作
  }

  return <p>示例一个延迟组件树</p>;
};

```



示例中为一个存在严重渲染性能问题的组件，在内部 `color`  发生变化时，都会重新渲染人为延迟组件树 ExpensiveTree 延迟非常慢的内容



> 解决方案一： useMemo



```tsx
import { useMemo, useState } from "react";

export const useMemoExample = () => {
  const [color, setColor] = useState<string>("white");

  const tree = useMemo(() => <ExpensiveTree />, []);

  return (
    <>
      <input
        value={color}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
          setColor(e.target.value)
        }
      />
      <p style={{ color }}>你好，世界！</p>
      {tree}
    </>
  );
};

export const ExpensiveTree = () => {
  const now = performance.now();

  while (performance.now() - now < 100) {
    // 在代码中进行人为延迟，在 100 ms 内不做任何操作
  }

  return <p>示例一个延迟组件树</p>;
};

```



通过包装 ExpensiveTree 确保它仅在其任何依赖性变化时才重新呈现。由于将空数组作为第二个参数传递给 `useMemo` ，ExpensiveTree 将在出事渲染期间仅被记忆和渲染一次



> 解决方案二：状态下移



代码中，真正渲染对返回树只关注当前：



```tsx
const [color, setColor] = useState<string>("white");

<input value={color} onChange={(e) => setColor(e.target.value)} />
<p style={{ color }}>你好，世界！</p >

```



因此，将部分提取到组件中，将状态下移到 From，实现改变颜色后，仅重新渲染



```tsx
import { useState } from "react";

export const useMemoExample = () => {
  return (
    <>
      <ExpensiveTree />
    </>
  );
};

export const ExpensiveTree = () => {
  const now = performance.now();

  while (performance.now() - now < 100) {
    // 在代码中进行人为延迟，在 100 ms 内不做任何操作
  }

  return <p>示例一个延迟组件树</p>;
};

export const Form = () => {
  const [color, setColor] = useState<string>("blue");

  return (
    <>
      <input
        value={color}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
          setColor(e.target.value)
        }
      />
      <p style={{ color }}>你好，世界！</p>
    </>
  );
};

```



> 解决方案三：提升内容



在解决方案二中，如果状态所属的代码用于昂贵树上的某个位置，上述解决方案二将不起作用。例如，假设我们将 color 放在父级 `<div />` 上：



```tsx
export const useMemoExample = () => {
  const [color, setColor] = useState<string>("white");

  return (
    <div style={{ color }}>
      <input
        value={color}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
          setColor(e.target.value)
        }
      />
      <p style={{ color }}>你好，世界！</p>
      <ExpensiveTree />
    </div>
  );
};

```



解决方案：



```tsx
import { useState } from "react";

export const useMemoExample = () => {
  return (
    <ColorPicker>
      <p>你好，世界！</p>
      <ExpensiveTree />
    </ColorPicker>
  );
};

export const ExpensiveTree = () => {
  const now = performance.now();

  while (performance.now() - now < 100) {
    // 在代码中进行人为延迟，在 100 ms 内不做任何操作
  }

  return <p>示例一个延迟组件树</p>;
};

export const ColorPicker = ({ children }: any) => {
  const [color, setColor] = useState<string>("white");

  return (
    <div style={{ color }}>
      <input
        value={color}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
          setColor(e.target.value)
        }
      />
      {children}
    </div>
  );
};

```



在解决方案中，尝试将组件一分为二：

* 依赖 color 和以及 color 状态本身变量移至 `<ColorPicker />`
* 不关心 color 到部分留在 App 组件中并作为 jsx 的内容传递给 `<ColorPicker />` ，称为 children prop



color 发生变化时，`<ColorPicker />` 重新渲染，Children 因为其身上仍具有上次获得相同的 props ，因此 React 不会访问该子树



> 关于意义



在应用 memo 或之类优化之前的 useMemo ，看看能否将更改的部分和未更改的部分是有意义的。上述提出的方阿福有趣的是：它们本身与性能没有任何关系。

使用 children 来拆分 props 拆分组件通常会使应用程序的数据流更易于遵循，并减少通过树向下查找的 props 数量。在这种情况下，提高性能只是锦上添花，而不是最终目标。

这种模式或许可以在未来某些调价下释放更多性能优势：

例如，当[服务器组件稳定并准备好采用时，我们的组件可以从服务器上的 `ColorPicker` 接收它。整个组件或其部分都可以在服务器上运行，甚至 React 状态更新也会 “跳过” 客户端上`children` `<ExpensiveTree />`的这些部分。

这是`memo`做不到的事情，但同样，这两种方法是互补的，**不要忽视向下移动状态（并向上提升内容）**