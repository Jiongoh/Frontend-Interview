# CSS篇

#### 1. CSS选择器及其优先级

|     选择器     |     格式      | 优先级权重 |
| :------------: | :-----------: | :--------: |
|    id选择器    |      #id      |    100     |
|    类选择器    |  .classname   |     10     |
|   属性选择器   | a[ref=“url”]  |     10     |
|   伪类选择器   | li:last-child |     10     |
|   标签选择器   |      div      |     1      |
|  伪元素选择器  |   li:after    |     1      |
| 相邻兄弟选择器 |     h1+p      |     0      |
|    子选择器    |     ul>li     |     0      |
|   后代选择器   |     li a      |     0      |
|  通配符选择器  |       *       |     0      |

注意事项：

- !important声明的样式的优先级最高
- 内联样式优先级权重为1000
- 如果优先级相同，则最后出现的样式生效
- 样式表的来源不同时，优先级顺序为：内联样式 > 内部样式 > 外部样式 > 浏览器用户自定义样式 > 浏览器默认样式

------

#### 2.  CSS中可继承与不可继承属性有哪些

- 可继承属性：
  1. 字体系列：font-family、font-weight、font-size、font-style
  2. 文本系列：text-indent、text-align、line-height、word-spacing、letter-spacing、text-transform、color
  3. 元素可见性：visibility
  4. 列表布局属性：list-style
  5. 光标属性

- 不可继承属性：
  1. display
  2. 文本属性：vertical-align、text-decoration、text-shadow、white-space
  3. 盒子模型的属性：width、height、margin、border、padding
  4. 背景属性：background、background-color、background-image
  5. 定位属性：float、clear、position、top、right、bottom、left、min-width、min-height、max-width、max-height、overflow、z-index
  6. 内容属性：content

------

#### 3. display的属性值及其作用

|    属性值    |                           作用                           |
| :----------: | :------------------------------------------------------: |
|     none     |             元素不显示，并且会从文档流中移除             |
|    block     |    块类型。默认宽度为父元素宽度，可设置宽高，换行显示    |
|    inline    | 行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示 |
| inline-block |        默认宽度为内容宽度，可以设置宽高，同行显示        |
|  list-item   |         像块类型元素一样显示，并添加样式列表标记         |
|    table     |                此元素会作为块级表格来显示                |
|   inherit    |           规定应该从父元素继承display属性的值            |

------

#### 4. display的block、inline和inline-block的区别

- block：会独占一行，多个元素会另起一行，可以设置width、height、margin和padding属性

- inline： 元素不会独占一行，设置width、height属性无效。但可以设置水平方向的margin和padding属性，不能设置垂直方向的padding和margin

- inline-block：将对象设置为inline对象，但对象的内容作为block对象呈现，之后的内联对象会被排列在同一行内

  > 注意：行内元素可以设置水平方向的margin和padding属性，不能设置垂直方向的padding和margin

------

#### 5. 隐藏元素方法

- `display: none`：直接从文档树中删除该节点
- `visibility: hidden`：元素在页面中仍占据空间，文档树中仍有该节点，但是不会响应绑定的监听事件
- `opacity: 0`：将元素的透明度设置为 0，以此来实现元素的隐藏。元素在页面中仍然占据空间，文档树中仍有该节点，并且能够响应元素绑定的监听事件
- `position: absolute`：通过使用绝对定位将元素移除可视区域内，以此来实现元素的隐藏
- `z-index: -9999`：来使其他元素遮盖住该元素，以此来实现隐藏
- `transform: scale(0,0)`：将元素缩放为 0，来实现元素的隐藏。这种方法下，元素仍在页面中占据位置，但是不会响应绑定的监听事件。

------

