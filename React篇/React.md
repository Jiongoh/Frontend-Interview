# React篇

#### 1. 对React的理解

是一个UI网页框架，核心思路是组件化、通用性和声明式。组件化的优势在于代码的复用；通用性的优势在于一次学习随处编写，比如写了一个React网页就可以使用React Native等打包成跨平台的应用，当然虚拟DOM是实现跨平台的保障；声明式的好处就是直观，更容易组合。

------

#### 2. 为什么React要用JSX

首先React并没有强制要求使用JSX，开发人员依然可以使用`React.createElement`API来创建React元素，而JSX更像是一种语法糖。

------

#### 3. React的事件机制：

React并没有将事件绑定到真实的DOM上，而是在根元素（16版本是document，17版本是root元素）处监听所有事件。当事件冒泡到document时，React会将事件统一包装成React Event，并交给真正的处理函数处理。

![ReactEvent](D:./ReactEvent.png)

------

#### 4. HOC和Hooks的区别

- 高阶组件（HOC）：是一种类似于Vue的Minix的复用组件逻辑的高级技巧，并不是React的API。简单来说就是定义一个函数，将组件作为参数传入，经过一系列逻辑处理，返回一个新的强化后的组件。

  ```jsx
  function withAuth(WrappedComponent) {
      return function WrappedComponent(props) {
          //    逻辑处理
          return <WrappedComponent {...props} />
      }
  }
  ```

  

- Hooks：主要在函数式组件中使用，通过使用官方已经写好的hook，可以让我们在使用函数式组件时仍然可以使用state以及其他的React的特性，例如`useState`、`useEffect`、`useMemo`等。还可以自定义hook，提高代码的复用率。

------

#### 5. 对React-Fiber的理解

在旧版本的React中，React会递归虚拟DOM树，在这个期间会一直占用主线程，导致浏览器的其他动作不能执行，也就是用户说的**卡顿**。

而React-Fiber就是为了解决这个问题而诞生的，React团队通过将虚拟DOM树拆分成多分，在浏览器空闲时间工作，分批延时对DOM进行操作，并且执行过程可以被中断，以用户的交互为最高级任务，随时将主线程交还给用户。

------

#### 6. 正常的函数式组件（React.Component）和用React.memo包裹的组件（React.PureComponent）的区别

React.memo（HOC）包裹的组件表示纯组件，其作用是当组件更新时，如果组件的props和state都没有改变，render函数不会调用，达到性能优化的目的。但需要注意的是React内部在对props和state前后比较时，使用的是**浅比较**，当数据类型是引用类型并数据发生改变时，组件可能不会正常更新。因此React.memo通常用在纯展示的组件上。

------

#### 7. 对getDerivedStateFromProps的理解

在该生命周期钩子内更新state不会触发render函数对组件进行重新渲染。

使用场景：可以在子组件的render函数执行前获取新的props，从而更新子组件自己的state。 可以将数据请求放在这里进行执行，需要传的参数则从getDerivedStateFromProps(nextProps)中获取，从而减轻请求负担。

------

#### 8. 类组件与函数组件的区别

- 类组件存在生命周期的概念，但是函数组件没用，因为函数组件本质上是函数，一旦开启执行就无法中断。但React也提供了useState、useEffect等hooks，让我们使用函数组件也能做到类组件才有的功能和概念。
- 类组件的设计模式是基础，模型是面向对象，而函数组件的内核是函数式编程，并且也是React官方推荐的未来趋势。

------

#### 9. 组件中的通信有哪些

父-->子：父组件在调用子组件时传入**props**

子-->父：子组件绑定事件，在事件内的**回调函数**中将信息作为参数传给父组件

兄弟间通信：一个兄弟组件使用回调函数将信息传给共同的父组件，父组件再通过props传给另一个兄弟组件；React Context

无关联组件间通信：引入状态管理框架Redux

------

#### 10. 哪些方法会触发 React 重新渲染，重新渲染 render 会做些什么

- setState等更新状态机的方法被调用
- 父组件只要被重新渲染，无论传入子组件的props有没有变化，子组件都一定会重新渲染
- render被调用就意味着React要对对两棵虚拟DOM树进行Diff算法对比，将有差异的节点进行替换

------

#### 11. 对React Fragment（<></>）的理解

在React中，组件返回的元素只能有一个根元素。为了不添加多余的DOM节点，我们可以使用Fragment标签来包裹所有的元素，Fragment标签不会渲染出任何元素

------

#### 12.  React如何获取组件对应的DOM元素

1. 使用`Raect.createRef`（类式）/`useRef`（函数式）API创建一个ref对象。例如：

   ```jsx
   const myRef=React.createRef();
   {/*或者*/}
   const myRef=useRef(null);
   const myRefs=useRef([]);{/*获取多个元素*/}
   ```

2. 将ref对象与组件内的DOM元素关联起来。例如：

   ```jsx
   <div ref={myRef}></div>
   {/*或者*/}
   <div ref={(ref)=>myRefs.push(ref)}></div>{/*获取多个元素*/}
   ```

3. 通过`myRef.current`获取到当前DOM元素

------

#### 13.  React中可以在render访问ref吗？为什么

不可以，render阶段就是在渲染DOM元素，此时真正的DOM元素还没有生成。

------

#### 14.  对React的插槽(Portals)的理解，如何使用，有哪些使用场景

- 理解：使用插槽创建的组件可以脱离父组件的层级，挂载在DOM树的任意位置。而父组件重新渲染时，子组件不会重新渲染，这也是一种性能优化手段

- 使用：

  使用`ReactDOM.createPortal(child,container)`API创建一个插槽组件

  第一个参数child是其他组件或者DOM元素

  第二个参数是包裹插槽的组件或者DOM元素

  ```jsx
  ReactDOM.createPortal(child,container);
  ```

  **记得在App中渲染插槽组件**

- 使用场景：对话框，遮罩层，弹出菜单等

------

#### 15. 如何避免不必要的render

- shouldComponentUpdate钩子中返回false
- 使用PureComponent创建类组件
- 使用React.memo创建函数组件

------

#### 16. 受控组件和非受控组件

- 受控组件：即表单输入类元素的值交给React管理，也就是元素的值与state绑定，通过事件更新来更新状态
- 非受控组件：使用ref绑定表单输入类元素，在事件的回调中再去通过ref获取DOM元素，进而获取其值

------

#### 17. setState和useState的工作原理

- setState：
  1. 调用`setState()`后，React会将新状态存在组件实例对象上
  2. 将新状态与之前的状态合并，形成最新的组件状态
  3. 组件状态更新后，React会将组件标记为'dirty'状态，将重新渲染的请求放入更新队列中
  4. 调用render方法重新渲染
- useState：
  1. 调用`useState(initialState)`后，React会创建一个状态变量和一个与之对应的状态更新函数
  2. 每当你调用React创建的状态更新函数，React会将新的状态值直接替换旧的状态值，并将组件标记为需要重新渲染
  3. 调用render方法重新渲染

------

#### 18. setState 是同步还是异步的

**setState有可能是同步的也可能是异步的**

- 同步情况：在自己绑定原生事件或定时器中，setState是同步的，可以在修改state后立刻拿到最新的值
- 异步情况：因为如果每一次修改state就重新渲染页面，这样对性能的开销是很大的。因此React会获取到多个状态后，再将将多个状态合并，再进行批量更新。在React可以控制的地方，例如生命周期或合成事件中，setState就是异步的。

------

#### 19. 组件中的默认props

- 在旧版类组件中，可以用getDefaultProps配置项来定义一个函数，其返回的对象就是默认的props
- 新版的类式组件中，可以用static声明一个对象，该对象即为默认的props
- 函数组件中，可以通过设置参数的默认值作为默认的props

------

#### 20. setState的第二个参数作用是什么

setState 的第二个参数是一个可选的回调函数。这个回调函数将在组件重新渲染后执行。等价于在 componentDidUpdate 生命周期内执行。在这个回调函数中你可以拿到更新后 state 的值

------

#### 21. 在React中组件的this.state和setState有什么区别

- this.state通常是用于初始化state的
- this.setState是用来修改state值的，如果直接修改state的值，而没有通过setState修改，React不会检测到修改，页面也不会更新

------

#### 22. state和props的异同

- 相同点：都能驱使页面的重新渲染
- 不同点：
  1. props是外部传入组件的参数，具有只读性
  2. state是组件自己控制的状态，是多变的，并且每次调用setState更新state都会触发组件重新渲染

------

#### 23. React中的props为什么是只读的

props类似于函数的形参，是组件间通信的接口，只能从父组件流向子组件

------

#### 24. 在React中组件的props改变时更新组件的有哪些方法

#### 调用getDerivedStateFromProps生命周期钩子，如果不需要props传入的内容影响到state，需要返回null。这个返回值是必须的尽量将其写到函数的末尾

------

#### 25. React中怎么检验props

- 引入PropTypes来验证。例如：

  ```jsx
  import PropTypes from 'prop-types';
  
  function Greeting(props) {
      return (
        <h1>Hello, {this.props.name}</h1>
      );
    }
  
  Greeting.propTypes = {
    name: PropTypes.string
  }
  ```

- 项目中使用TS

------

#### 26. React的生命周期

分为三个阶段：挂载阶段、更新阶段、卸载阶段

- 挂载阶段：`constructor()`初始化状态和绑定方法-->`getDerivedDefaultProps()`组件收到props时调用-->`render()`渲染UI，返回一个React元素-->`componentDidMount()`组件首次挂载时调用
- 更新阶段：`getDefaultProps()`组件收到props时调用-->`shouldComponentUpdate()`判断是否需要重新渲染组件，然后false会中断流程-->`render()`渲染UI，返回一个React元素-->`componentDidUpdate()`组件更新完时调用
- 卸载阶段：`componentWillUnmount()`组件卸载前调用

------

#### 27. React 性能优化在哪个生命周期？它优化的原理是什么？

react的父级组件的render函数重新渲染会引起子组件的render方法的重新渲染。但是，有的时候子组件的接受父组件的数据没有变动。子组件render的执行会影响性能，这时就可以使用shouldComponentUpdate生命周期钩子返回false来解决这个问题。

------

#### 28. state 和 props 触发更新的生命周期分别有什么区别？

相对于 state 更新，props 更新后唯一的区别是增加了对 componentWillReceiveProps 的调用。

------

#### 29. React中发起网络请求应该在哪个生命周期中进行？

对于异步请求，最好放在componentDidMount中去操作

------

#### 30. React-Router的实现原理（和Vue-Router原理一样）

- hash模式：通过监听`hashchange`事件，监听location.hash的变化，使用的组件是`<BrowserRouter/>`
- history模式：基于H5的新API history， 通过 history.pushState 和 resplaceState 等修改路由路径，会将url压入堆栈，同时也可以使用 `history.go()` 等 API自由跳转，使用的组件是`<HashRouter>`

------

#### 31. 如何配置 React-Router 实现路由切换

- 使用`<Route>`组件：

  ```jsx
  <Route path='/about' component={About}/>
  ```

- 使用 `<Link>`、`<NavLink>`、`<Redirect>` 组件：

  ```jsx
  <Link to="/">Home</Link>
  ```

  ```jsx
  <NavLink to="/react" activeClassName="hurray">
      React
  </NavLink>
  ```

  ```jsx
  <Redirect/>
  ```

- 路由表：

  ```jsx
  {/*配置*/}
  const route=[
  	{
  		path:'/',
  		element:<Home/>
  	},
  	{
  		path:"/about",
  		element:<About/>
  	}
  ]
  
  {/*使用*/}
  <BrowserRouter>
  	<Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
  </BrowserRouter>
  ```

------

#### 32. React-Router怎么设置重定向

- 使用`<Redirect/>`组件：

  ```jsx
  <Redirect to="/home" />
  ```

- 使用history对象：

  ```jsx
  const history = useHistory();
  history.push("/home")
  ```

------

#### 33.  react-router 里的 Link 标签和 a 标签的区别

从最终渲染的 DOM 来看，这两者都是链接，都是a标签。

区别是：`<Link>`是react-router 里实现路由跳转的链接，其内部阻止了a标签的默认事件，一般配合`<Route>` 使用，react-router接管了其默认的链接跳转行为，区别于传统的页面跳转，`<Link>` 的“跳转”行为只会触发相匹配的`<Route>`对应的页面内容更新，而不会刷新整个页面。

------

#### 34. React-Router如何获取URL的参数

- 获取查询参数：

  1. 使用`useLocation()`钩子：

     ```jsx
     const location = useLocation();
     const quertParams = new URLSearchParams(location.search);
     const paramValue = queryParams.get('params');
     ```

  2. 使用`this.props.location`获取（适用于类组件）：

     ```jsx
     const quertParams = new URLSearchParams(this.props.location.search);
     const paramValue = queryParams.get('params');
     ```

- 动态路由传值：

  1. 使用`useParams()`钩子：

     ```jsx
     {/* 加入路径是/user/:id */}
     const { id } = useParmas();
     ```

  2. 使用`this.props.match.params`获取（适用于类组件）：

     ```jsx
     const { id } = this.props.match.params;
     ```

------

#### 35. 
