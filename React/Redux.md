# Redux

[**Redux 中文官网**](https://cn.redux.js.org/)

## 安装 Redux Toolkit 和 React-Redux

```
npm install @reduxjs/toolkit react-redux
```



## 教程目录

- [**Redux 基础教程**](https://cn.redux.js.org/tutorials/essentials/part-1-overview-concepts) 是一个“自上而下”的教程，使用推荐的最新 API 和最佳实践来教“如何以正确的方式使用 Redux ”
- [**Redux 深入浅出**](https://cn.redux.js.org/tutorials/fundamentals/part-1-overview) 是一个“自下而上”的教程，教程从最初的原理开始，没有任何抽象地讲授“ Redux 工作原理”，以及 Redux 标准规范存在背后的原因

## Redux 文件常用命名和文件结构规定

1. Actions：
   * 操作负责定义可以在应用程序中分派的不同类型操作
   * 文件名： actionTypes.js ( action.js )
2. Actions Creators：
   * 动作创建者是创建和返回的动作的函数
   * 文件名： actionCreator.js
3. Reducers：
   * 减速器是指定应用程序的状态如何响应操作而变化的函数
   * 文件名： reducer.js
4. Store Configuration：
   * 文件设置 Redux 存储组合减速器
   * 文件名： store.js
5. Redux Middleware：
   * 如果在 Redux 存储中使用中间件，请将中间件配置放在单独的文件中
   * 文件名： middleware.js （如果只有一个中间件）或 middlewares.js （如果拥有多个中间件）
6. Selectors：
   * 选择器是从 Redux 存储中提取特定状态片段的函数
   * 文件名： selectors.js 
7. Constants：
   * 如果在你的应用程序中具有和 Redux 相关的特定常量，例如初始状态值或操作名称，可以将它们存储在单独的文件中
   * 文件名： constants.js



以下为目录结构示例：

```markdown
src/
  - actions/
    - actionTypes.js
    - actionCreators.js
  - reducers/
    - reducers.js
  - store.js
  - middleware.js
  - selectors.js
  - constants.js

```



## 使用示例：

> 写例包含组件 A 传值 （ title ， context ）给组件 B 的过程，示例将遵循通用的项目文件夹结构和文件命名规范

项目文件夹结构：

```markdown
project/
  |- src/
  |   |- components/
  |   |   |- ComponentA.tsx
  |   |   |- ComponentB.tsx
  |   |- redux/
  |   |   |- actions/
  |   |   |   |- types.ts
  |   |   |   |- componentActions.ts
  |   |   |- reducers/
  |   |   |   |- componentReducer.ts
  |   |   |- store.ts
  |   |- App.tsx
  |   |- index.tsx

```



**src/components/ComponentA.tsx:**

```tsx
import React from 'react';
import { useDispatch } from 'react-redux';
import { setComponentData } from '../redux/actions/componentActions';

interface ComponentAProps {
  title: string;
  context: string;
}

const ComponentA: React.FC<ComponentAProps> = ({ title, context }) => {
  const dispatch = useDispatch();

  const sendDataToComponentB = () => {
    dispatch(setComponentData({ title, context }));
  };

  return (
    <div>
      <h2>{title}</h2>
      <p>{context}</p>
      <button onClick={sendDataToComponentB}>Send Data to Component B</button>
    </div>
  );
};

export default ComponentA;

```



**src/components/ComponentB.tsx:**

```tsx
import React from 'react';
import { useSelector } from 'react-redux';
import { RootState } from '../redux/reducers/componentReducer';

const ComponentB: React.FC = () => {
  const { title, context } = useSelector((state: RootState) => state.component);

  return (
    <div>
      <h2>Component B</h2>
      <h3>{title}</h3>
      <p>{context}</p>
    </div>
  );
};

export default ComponentB;

```



**src/redux/actions/types.ts:**

```tsx
export const SET_COMPONENT_DATA = 'SET_COMPONENT_DATA';
```



**src/redux/actions/componentActions.ts:**

```tsx
import { SET_COMPONENT_DATA } from './types';

export const setComponentData = (data: { title: string; context: string }) => {
  return {
    type: SET_COMPONENT_DATA,
    payload: data,
  };
};

```



**src/redux/reducers/componentReducer.ts:**

```tsx
import { SET_COMPONENT_DATA } from '../actions/types';
import { AnyAction } from 'redux';

export interface ComponentState {
  title: string;
  context: string;
}

const initialState: ComponentState = {
  title: '',
  context: '',
};

const componentReducer = (state = initialState, action: AnyAction) => {
  switch (action.type) {
    case SET_COMPONENT_DATA:
      return {
        ...state,
        title: action.payload.title,
        context: action.payload.context,
      };
    default:
      return state;
  }
};

export default componentReducer;

```

在最新版本的Redux中，`createStore` 函数的用法略有变化，不再直接从 `redux` 模块中导入。相反，你需要从 `redux` 中导入 `configureStore` 函数，它取代了 `createStore` ，并提供了更多的功能



**src/redux/store.ts:**

```tsx
import { configureStore } from '@reduxjs/toolkit';
import componentReducer from './reducers/componentReducer';

const store = configureStore({
  reducer: {
    component: componentReducer,
  },
});

export default store;

```



**src/App.tsx:**

```tsx
import React from 'react';
import { Provider } from 'react-redux';
import ComponentA from './components/ComponentA';
import ComponentB from './components/ComponentB';
import store from './redux/store';

const App: React.FC = () => {
  return (
    <Provider store={store}>
      <div>
        <ComponentA title="Hello" context="This is Component A" />
        <ComponentB />
      </div>
    </Provider>
  );
};

export default App;

```



**src/index.tsx:**

```tsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));

```

示例展示了如何在 React 应用中使用 Redux 来在 ComponentA 中传递数据（title 和 context），并在ComponentB 中获取数据。通过 dispatch 一个 action 并在 Reducer 中更新状态，实现数据的传递和共享



## 关于传递数据使用 Redux 还是 Context

### Redux : 

> 优势

1. 全局状态管理：Redux 将状态集中管理，使得多个组件可以轻松共享数据，避免了深层嵌套组件传递 props 的问题
2. 可预测的状态变化：Redux 的数据流是**单向的**，通过 reducer 修改状态，使得状态变化变得可控和可预测
3. 开发者工具支持：Redux 提供强大的开发者工具，可以方便调试状态变化，记录状态历史等
4. 中间件：可以使用中间件处理异步操作、日志记录等，提高代码的可维护性



> 劣势

1. 设置繁琐：相对于 Context，Redux 的设置和使用需要更多的代码，特别在小型应用时显得冗余
2. 学习成本高：Redux 有一定学习曲线，对于新手而言，理解 Redux 工作原理和概念需要时间



> 使用场景

* 大型应用，需要全局状态管理
* 需要将状态和状态变化与组件**解耦**，使得组件更易于测试和维护



### Context : 

> 优势

1. 轻量级：相对于 Redux，Context 的设置和使用相对简单，适用于小型应用或局部状态管理
2. 不需要第三方库：Context 是 React 自带的功能，不需要额外的第三方库



> 劣势

1. 不适合大型应用：Context 虽然可以解决状态共享的问题，单当应用变得庞大且复杂时，Context 可能会变得难以维护
2. 不易于测试：相对于 Redux 的开发者工具，Context 提供的调试功能较为有限



> 适用场景

* 小型应用，或者局部状态管理
* 简单的状态共享，不需要复杂的中间件或状态管理工具



综上所述，如果应用较大且需要全局状态管理、可预测的状态变化以及复杂的状态处理逻辑，那么 Redux 是更合适的选择。但如果应用相对较小且状态共享较为简单，或者希望简化设置和代码，Context 是一个不错的选择。在某些情况下，你甚至可以将两者结合使用，根据实际情况进行灵活选择