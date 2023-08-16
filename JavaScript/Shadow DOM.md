# Shadow DOM

## 什么是 Shadow DOM

Shadow DOM（影子 DOM）是一种浏览器技术，用于创建封装的组件化 DOM 树。它允许你将一部分 DOM 树和样式封装在一个隔离的作用域内，这样可以避免外部样式和JavaScript代码影响到内部的组件。Shadow DOM 是 Web 组件技术的一部分，能够帮助开发者构建更加模块化和可重用的前端组件



上述图片中，可以预见 `input` 其实也附加了 Shadow DOM，比如，在 Chrome 中尝试给一个 Input 加上 `placeholder` ，通过 DevTools 便能看到，其实文字是在 ShadowRoot 下的一个 Id 为 `palcehoder` 的 div 中

### Google 浏览器查看标签属性内部结构（ Shadow DOM ）

*Settings > Preferences > Elements > Show user agent shadow DOM*

### Shadow DOM 结构树

Shadow DOM 允许将隐藏的 DOM 树附加到常规的 DOM 树中 —— 它以 shadow root 节点为起始根节点，在这个根节点的下方，可以是任意元素，和普通的 DOM 元素一样

Shadow DOM 允许在文档（ Document ）渲染时插入一棵「子 DOM  树」，并且这棵子树不在主 DOM 树中，同时为子树中的 DOM 元素和 CSS 提供了封装的能力。Shadow DOM 使得子树 DOM 与主文档的 DOM 保持分离，子 DOM 树中的 CSS 不会影响到主 DOM 树的内容，如下图所示：

![image](https://github.com/htllog/StudyNotes/assets/118370026/1feffb1d-5ed7-48db-9e33-1bd485086f13)

图示注解如下 ⬇️ 


### Shadow DOM 相关技术概念

* Shadow host：一个常规 DOM 节点，Shatdow DOM 会被附加到这个节点上
* Shadow tree：Shatdow DOM 内部的 DOM 树
* Shadow boundary：Shadow DOM 结束的地方，也是常规 DOM 开始的地方
* Shadow root：Shadow tree 的根节点 

## 性能优势 - 组件隔离（ Shadow DOM ）

Shadow DOM 为自定义组件提供包括 CSS 、事件的有效隔离，不用担心不同的组件直接的样式、事件污染，相当于为自定义组件提供天然有效的保护伞

Shadow DOM 实际上是一个独立的子 DOM Tree，通过有限的接口的外部发生作用。都知道页面中的 DOM 节点数越多，运行时性能将会越差。这是因为 DOM 节点的相互作用会时常触发重给（ Repaint ）和重排（ reflow ）时会关联计算大量 Frame 关系

应用中，对 CSS 的隔离也会加快选择器的匹配速度，即使可能是微妙级的提升，但是在极端的性能情况下，仍然是有效的手段

Shadow DOM 最大的用处应该是隔离外部环境用于封装组件。估计浏览器的开发者们也意识到通过 HTML / CSS 来实现浏览器内建的原生组件更容易，如上边提到的浏览器原生组件 `input`，`video`，还有 `textarea`，`select`，`audio` 等，也都是由 HTML/CSS 渲染出来的

## 主流浏览器的支持情况

![image](https://github.com/htllog/StudyNotes/assets/118370026/a99760eb-5a89-4db5-a8cc-df1030fcd006)

各浏览器支持详细情况，请参考如下链接：

https://caniuse.com/ 

## 可以附加 Shadow DOM 的元素

### 导致 DOMException 错误

并非所有 HTML 元素可以开启 Shadow DOM，只有有限的元素可以附加 Shadow DOM。有时尝试将 Shadow DOM 树附加到某些元素是会导致 `DOMException`  的错误，例如：

```javascript
document.createElement('img').attachShadow({mode: 'open'}); 

const input = document.querySelector('input'
const inputRoot = input.attachShadow({mode: 'open'})

// => DOMException
```

提供的代码将引发 `DOMException` 错误，并显示消息“无法在 Element 上执行 attachShadow：无法在 void 或 select 元素上创建 Shadow root 

![image](https://github.com/htllog/StudyNotes/assets/118370026/90d19450-0414-40d3-9a5f-6a42075aa3b0)


### 支持的元素

任何有效的名称且可独立存在的自定义元素 && 下表：

|  article   |    aside    | blockquote |
| :--------: | :---------: | :--------: |
|  **Body**  |   **div**   | **footer** |
|   **h1**   |   **h2**    |   **h3**   |
|   **h4**   |   **h5**    |   **h6**   |
| **header** |  **main**   |  **nav**   |
|   **p**    | **section** |  **span**  |

## Shadow DOM 应用场景

1. **Web 组件库：** 如果你正在开发一个 Web 组件库，希望提供独立、可定制的 UI 组件，那么使用 Shadow DOM 可以有效隔离每个组件的样式和结构，避免组件之间的样式冲突，同时也可以提供更清晰的组件接口
2. **隔离第三方组件：** 当你在项目中使用第三方组件时，这些组件的样式和结构可能会与你的项目发生冲突。使用 Shadow DOM 可以在你的组件内部隔离第三方组件，避免影响整体样式
3. **定制化的样式：** 如果你想为某个特定部分的页面或应用定制化样式，但又不希望影响其他部分，可以使用 Shadow DOM 来创建一个封闭的容器，应用定制化的样式规则
4. **小部件或插件：** 对于一些小型的部件或插件，使用 Shadow DOM 可以将它们的样式和行为封装起来，防止与页面中的其他元素发生冲突
5. **独立的小应用：** 如果你正在构建一个独立的小应用，使用 Shadow DOM 可以帮助你隔离应用的样式和结构，使其更加独立和可维护
6. **保护敏感信息：** 如果你的应用中包含一些敏感信息，可以使用 Shadow DOM 来隔离这些信息，防止恶意脚本访问
7. **在不同环境中使用相同组件：** 使用 Shadow DOM 可以帮助你在不同的 Web 环境中使用相同的组件，而不必担心样式和结构冲突

## Shadow DOM 应用实例

### 如何创建 Shadow DOM

示例：

```html t
<html>
  <head>
    <title>创建 Shadow 示例</title>
  </head>
  <body>
    <h2>Shadow Example</h2>
    <div id="host" />
    <script>
      const host = document.querySelector("#host");
      // 通过 attachShadow 向元素附加 Shadow DOM

      const shodowRoot = host.attachShadow({ mode: "open" });
      // 向 shodowRoot 中添加一些内容

      shodowRoot.innerHTML = `<style>*{color:green;}</style><h3>自定义 Shadow DOM 中的样式，不影响主文档中的元素</h3>`;
    </script>
  </body>
</html>
```



`Element.attachShadow`  的参数 `shadowRootInit`  的 `mode`  选项用于设定封装模式,它有两个可选的值 ：

- **"open"** ：可 Host 元素上通过 `host.shadowRoot`  获取 shadowRoot 引用，这样任何代码都可以通过 shadowRoot 来访问的子 DOM 树
- **"closed"**：在 Host 元素上通过 `host.shadowRoot`  获取的是 null，我们只能通过 `Element.attachShadow` 的返回值拿到 shadowRoot 的引用（通常可能隐藏在类中）。例如，浏览器内建的 input、video 等就是关闭的，我们没有办法访问它们



### 简单自定义按钮组件

使用 Shadow DOM 的步骤如下：

1. **创建 Shadow Root：**在一个元素上使用 `element.attachShadow({ mode: 'open' })` 方法来创建一个 Shadow Root（影子根）。`mode` 参数可以是 `open` 或 `closed`

   `open` 允许外部访问 Shadow DOM，而 `closed` 则不允许

2.   **将内容插入 Shadow DOM：**将组建的结构和样式通过 DOM 操作插入到 Shadow DOM 中。可以使用 Shadow DOM 提供的 DOM 方法来操作内部的 DOM

3. **设置样式和样式作用域：**在 Shadow DOM 中，样式只影响组建内部。可以在 Shadow DOM 内部定义样式，并确保这些样式只应用于 Shadow DOM 内部



下为如何使用 Shadow DOM 创建一个简单的自定义按钮：

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      /* 外部样式不会影响 Shadow DOM 内部 */
      button {
        color: red;
      }
    </style>
  </head>
  <body>
    <my-button></my-button>
    <button>按钮样式</button>

    <script>
      customElements.define(
        "my-button",
        class extends HTMLElement {
          constructor() {
            super();
            const shadowRoot = this.attachShadow({ mode: "open" });

            const button = document.createElement("button");
            button.textContent = "Click Me";

            // 在 Shadow DOM 中设置样式
            const styles = `button {
              color: white;
              background-color: lightgray;
            }`;

            shadowRoot.innerHTML = `<style>${styles}</style>`;

            // 将按钮插入 Shadow DOM
            shadowRoot.appendChild(button);
          }
        }
      );
    </script>
  </body>
</html>
```

在上述示例中，通过自定义元素 `customElements.define` 定义了一个名为 `my-button` 的自定义按钮组件。在组件的构造函数中，创建了一个 Shadow Root，并在其中插入了一个按钮。按钮的样式定义在了 Shadow DOM 内部，不受外部样式的影响

**Tips：**Shadow DOM 只是 Web 组件技术的一部分，用于创建封装和隔离的组件。在实际应用中，可以将 Shadow DOM 与其他 Web 组件技术（如 Custom Elements 和 HTML Templates）一起使用，以构建更加模块化和可维护的前端组件

### Shadow DOM 在 React 使用

Shadow DOM 可以与 React 一起使用，但需注意以下**问题**和**注意事项**：

React 本身并不直接支持 Shadow DOM。但可以在 React 组件中使用 Shadow DOM，以便在组件内部实现封装和隔离

1. **自定义元素和 Web 组件：**React 组件可以被包装在自定义元素内，然后在该自定义元素上创建 Shadow DOM。这样可以在 React 组件内部使用 Shadow DOM 技术，将样式和结构封装
2. **组件封装和隔离：**使用 Shadow DOM 可以在 React 组件内部实现封装和 DOM 结构的隔离。对于构建可重用的组件来说，可以避免外部组件和样式冲突
3. **注意样式冲突：**使用 Shadow DOM 可以避免外部样式影响内部，但也可能导致一些样式冲突。在设计和使用时需注意，确保组件在不同上下文能正常使用
4. **React 渲染机制：**React 的渲染机制是基于虚拟 DOM，Shadow DOM 也维护独立的 DOM 树。结合使用过程中，需要注意 React 渲染和 Shadow DOM 的更新机制，避免出现不一致的情况



```tsx
import React, { useEffect, useRef } from "react";

export const ShadowDOMComponent = () => {
  const shadowRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const shadowRoot = shadowRef.current?.attachShadow({ mode: "open" });

    if (shadowRoot) {
      const styles = `
        p {
          color: green;
        }
      `;

      const pElement = document.createElement("p");

      pElement.textContent = "This is a Shadow DOM paragraph.";

      shadowRoot.innerHTML = `
        <style>${styles}</style>
      `;

      shadowRoot.appendChild(pElement);
    }
  }, []);

  return (
    <>
      <h2>React Component</h2>
      <div ref={shadowRef}></div>
    </>
  );
};
```



### React 结合 Shadow DOM 保护敏感信息实现

涉及敏感信息时，结合 React 和 Shadow DOM 可以增加一定安全性。在例子中，演示使用 React 创建一个包含敏感信息组件，将其封装在 Shadow DOM 内部，以隔离样式和结构，从而增强保护性

```tsx
import React, { useRef, useEffect } from "react";

interface SensitiveInfoComponentProps {
  sensitiveData: string;
}

const SensitiveInfoComponent = ({
  sensitiveData,
}: SensitiveInfoComponentProps) => {
  // 创建一个 ref，用于引用 Shadow DOM 容器
  const shadowRef = useRef<HTMLDivElement | null>(null);

  useEffect(() => {
    // 使用 useEffect，在组件挂载时执行以下逻辑
    if (shadowRef.current) {
      const shadowRoot = shadowRef.current.attachShadow({ mode: "open" });
      // 创建一个包含敏感信息的段落元素
      const container = document.createElement("div");
      container.innerHTML = `<p>This is sensitive information: ${sensitiveData}</p>`;
      // 将包含敏感信息的元素添加到 Shadow DOM 中
      shadowRoot.appendChild(container);
    }
  }, [sensitiveData]);

  return (
    <div>
      <h2>Protected Sensitive Information</h2>
      {/* 使用 ref 将 Shadow DOM 容器引用绑定到组件 */}
      <div ref={shadowRef}></div>
    </div>
  );
};

export const ProjectInformation = () => {
  return (
    <div>
      <h1>React with Shadow DOM</h1>
      {/* 渲染 SensitiveInfoComponent，传递敏感信息 */}
      <SensitiveInfoComponent sensitiveData="Confidential Data" />
    </div>
  );
};
```

这段代码实现了在 React 中使用 Shadow DOM 来保护敏感信息的示例。当 `SensitiveInfoComponent` 组件加载时，它会将包含敏感信息的元素放置在 Shadow DOM 内部，从而将其与主页面的其他内容隔离开来，增加了保护性和安全性

