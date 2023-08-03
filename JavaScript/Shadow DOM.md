# Shadow DOM

## 什么是 Shadow DOM



### Shadow DOM 结构树

*Shadow* DOM 允许将隐藏的 DOM 树附加到常规的 DOM 树中——它以 shadow root 节点为起始根节点，在这个根节点的下方，可以是任意元素，和普通的 DOM 元素一样

Shadow DOM 允许在文档（ Document ）渲染时插入一棵「子 DOM  树」，并且这棵子树不在主 DOM 树中，同时为子树中的 DOM 元素和 CSS 提供了封装的能力。Shadow DOM 使得子树 DOM 与主文档的 DOM 保持分离，子 DOM 树中的 CSS 不会影响到主 DOM 树的内容，如下图所示：

![image](https://github.com/htllog/StudyNotes/assets/118370026/a021e27e-ee71-464c-bc8d-d3cc6bcffe30)




### Shadow DOM 相关技术概念

* Shadow host：一个常规 DOM 节点，Shatdow DOM 会被附加到这个节点上
* Shadow tree：Shatdow DOM 内部的 DOM 树
* Shadow boundary：Shadow DOM 结束的地方，也是常规 DOM 开始的地方
* Shadow root：Shadow tree 的根节点 

## 性能优势 - 组件隔离（ Shadow DOM ）

Shadow DOM 为自定义组件提供包括 CSS 、事件的有效隔离，不用担心不同的组件直接的样式、事件污染，相当于为自定义组件提供天然有效的保护伞

Shadow DOM 实际上是一个独立的子 DOM Tree，通过有限的接口的外部发生作用。都知道页面中的 DOM 节点数越多，运行时性能将会越差。这是因为 DOM 节点的相互作用会时常触发重给（ Repaint ）和重排（ reflow ）时会关联计算大量 Frame 关系

应用中，对 CSS 的隔离也会加快选择器的匹配速度，即使可能是微妙级的提升，但是在极端的性能情况下，仍然是有效的手段

## 主流浏览器的支持情况



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

### 支持的元素

任何有效的名称且可独立存在的自定义元素 && 下表：

|  article   |    aside    | blockquote |
| :--------: | :---------: | :--------: |
|  **Body**  |   **div**   | **footer** |
|   **h1**   |   **h2**    |   **h3**   |
|   **h4**   |   **h5**    |   **h6**   |
| **header** |  **main**   |  **nav**   |
|   **p**    | **section** |  **span**  |

