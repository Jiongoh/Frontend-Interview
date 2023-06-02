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

#### 35. 对Redux的理解

Redux 提供了一个叫 store 的统一仓储库，组件通过 dispatch 将 state 直接传入store，不用通过其他的组件。并且组件通过 subscribe 从 store获取到 state 的改变。使用了 Redux，所有的组件都可以从 store 中获取到所需的 state，他们也能从store 获取到 state 的改变。

------

#### 36. Redux 原理及工作流程

- 原理：Redux的原理基于三个概念——state、action、reducer
  1. state：整个应用的状态都存储在一个js对象中，即"store"
  2. action：描述发生事件类型的普通js对象，用于通知应用程序状态的变化
  3. reducer：是一个纯函数，接收当前状态（state）和新动作（action），然后计算并返回新的状态。
- 工作流程：
  1. 组件通过dispatch方法将类型（type）和储存当前状态的载荷（payload）的action发送给store
  2. store接收action并将action转发给reducer
  3. reducer根据action的type对状态进行更新并将新状态返回给store
  4. store将新状态替换掉旧状态，并通知所有订阅该状态的组件更新

------

#### 37. Redux中的异步请求

使用react-thunk中间件（RTK中默认开启）

- 使用原生redux-thunk：

  1. 配置中间件，在store的创建中配置

     ```js
     import {createStore, applyMiddleware, compose} from 'redux';
     import reducer from './reducer';
     import thunk from 'redux-thunk'
     // 设置中间件
     const enhancer = composeEnhancers(
       applyMiddleware(thunk)
     );
     
     const store = createStore(reducer, enhancer);
     
     export default store;
     ```

  2. 创建异步的action函数

     ```js
     // 不再分发普通的action对象，而是分发异步的action函数
     const fetchData = () => {
         return (dispath) => {
             // 发送请求初始化的action
             dispath({
                 type: "pending"
             })
             axios.get('/api/data').then(response => {
                 dispath({
                     // 分发请求成功的action
                     type: "fulfilled",
                     payload: response.data
                 })
             }).catch(err => {
                 // 分发请求失败的action
                 dispath({
                     type: "rejected",
                     payload: err.message
                 })
             });
     
         }
     }
     ```

  3. reducer中正常处理

  4. 在组件中使用dispatch分发异步的action函数，而不是普通的action对象

     ```js
     import { fetchData } from "./userActions"
     const dispatch = useDispatch();
     dispatch(fetchData())
     ```

- 使用redux toolkit：

  1. 引入RTK相关函数和库，将独立的reducer模块配置入store中

     ```js
     import { createSlice, configureStore } from '@reduxjs/toolkit';
     const store = configureStore({
         reduer:{
             data:dataSlice.reducer
         }
     })
     ```

  2. 使用createAsyncThunk函数发送异步请求，该函数传入两个参数，第一个是action的type，第二个异步的函数，返回值是异步的action函数

     ```js
     import { createAsyncThunk } from '@reduxjs/toolkit';
     const fetchData = createAsyncThunk('request',
         async () => {
             const response = await fetch('/api/data');
             const data = await response.json();
             return data;
         }
     );
     ```

  3. slice中的reducer可以根据普通Promise对象状态的关键词来确定此时的action的类型，分别处理state。

  4. 在组件中使用dispatch分发异步的action函数，而不是普通的action对象

     ```js
     import { fetchData } from "./userActions"
     const dispatch = useDispatch();
     dispatch(fetchData())
     ```

------

#### 38. Redux 中间件是什么

Redux中间件通常是一个函数，提供的是位于 action 被发起之后，到达 reducer 之前的扩展点，也就是说store将action发送到reducer之前的一个时间点，在这一环节可以做一些"副作用"的操作，如异步请求、打印日志等。

------

#### 39. Redux和变量挂载到 window 中有什么区别

- 可维护性：直接将变量挂载到window上，使得变量的来源和用途不明确，导致代码难以理解和维护；而redux提供了统一的状态管理机制，让状态的变更和访问变得更加可控
- 命名冲突：window对象是一个全局的命名空间，很可能会发送变量名冲突的问题，导致错误和意外的发生；而在redux中通过派发action触发状态更新，通过reducer处理更新，这种明确的数据流动分享有利于追踪和调试
- Time travel调试：使用window存储状态，无法追踪状态变化的来源；而redux支持状态回溯调试，通过Redux DevTools可以查看每一次action的派发的时间点和状态的变化，对于调试应用有很大的帮助

------

#### 40. Redux和Vuex的异同

- 相同点：
  1. 单一数据来源：都遵循一个原则，就是把应用程序的状态存在一个全局的对象中
  2. 不可变性：都鼓励使用不可变的数据，即创建新的对象或数组来替换之前数据，而不是直接修改之前的数据
- 不同点：
  1. Vuex改进了Redux中的Action和Reducer函数，简化了reducer的流程，无需switch，只需在对应的mutation函数里改变state值即可（RTK中的reducer的设计类似于Vuex的mutation）
  2. Vuex由于Vue自动重新渲染的特性，只要生成新的State即可，组件会自动更新数据

------

#### 41. Redux的connect的作用

connect负责连接React和Redux

- 获取state：connect 通过 context获取 Provider 中的 store，通过` store.getState()` 获取整个store tree 上所有state
- 本质上是HOC，通过接收mapStateToProps和mapDispatchToProps两个参数将Redux的状态和行为以props的形式传递给React组件
- 监听state tree：connect缓存了store tree中state的状态，通过当前state状态 和变更前 state 状态进行比较，从而确定是否调用 `this.setState()`方法触发Connect及其子组件的重新渲染

------

#### 42. Hooks的原理及使用场景

- 原理：hooks基于函数组件和闭包，当函数组件被调用时，react会创建Fiber的数据结构来表示组件的状态和组件树结构，hooks使用fiber结构来管理状态和副作用
- 使用场景：React官方提供了一些钩子函数（如useState、useEffect等）来声明组件状态以及函数组件的生命周期相关的副作用，开发者还可以自定义hook满足各自的需求

------

#### 43. 为什么 useState 要使用数组而不是对象

先来看看使用数组和使用对象写发的话应该怎么写：

```jsx
const [count, setCount] = useState(0)
```

```jsx
const { state, setState } = useState(false);
const { state: counter, setState: setCounter } = useState(0) 
```

这关系到了ES6的解构赋值，如果 useState 返回的是数组，那么使用者可以对数组中的元素命名，代码看起来也比较干净；如果 useState 返回的是对象，在解构对象的时候必须要和 useState 内部实现返回的对象同名，想要正常使用的话，必须得设置别名才能使用返回值

------

#### 43. Hooks解决了哪些问题？

- 在组件之间复用状态逻辑很难：React 没有提供将可复用性行为"附加”到组件的途径，解决此类问题可以使用 render props 和 高阶组件。但是这类方案需要重新组织组件结构，这可能会很麻烦，并且会使代码难以理解。
- 复杂组件变得难以理解：在组件中，每个生命周期常常包含一些不相关的逻辑。例如，组件常常在 componentDidMount 和 componentDidUpdate 中获取数据。但是，同一个 componentDidMount 中可能也包含很多其它的逻辑，如设置事件监听，而之后需在 componentWillUnmount 中清除。相互关联且需要对照修改的代码被进行了拆分，而完全不相关的代码却在同一个方法中组合在一起。如此很容易产生 bug，并且导致逻辑不一致。
- 难以理解的 class：class 是学习 React 的一大屏障。我们必须去理解 JavaScript 中 this 的工作方式，这与其他语言存在巨大差异。还不能忘记绑定事件处理器。

------

#### 44. React Hook 的使用限制有哪些？

- 不要在循环、条件或嵌套函数中调用 Hook：因为 Hooks 的设计是基于数组实现。在调用时按顺序加入数组中，如果使用循环、条件或嵌套函数很有可能导致数组取值错位，执行错误的 Hook。当然，实质上 React 的源码里不是数组，是链表。
- 只能在函数组件中使用Hooks：Hooks的实现基于js中函数的闭包，但在类组件中没有这种概念。

------

#### 45. useEffect 与 useLayoutEffect的异同

- 相同点：都React定义的Hooks，使用方式都一样，都是用于处理副作用，包括修改DOM、设置订阅、操作定时器等

- 不同点：

  1. 使用场景：useEffect 在React的渲染过程中是被异步调用的，用于绝大多数场景；而useLayoutEffect会在所有的DOM变更之后同步调用，主要用于处理 DOM 操作、调整样式、避免页面闪烁等问题。但也正因为是同步处理，所以需要避免在 useLayoutEffect 做计算量较大的耗时任务从而造成阻塞。
  2. 执行时机：useEffect的执行时机是浏览器渲染完成之后，而useLayoutEffect的执行时机是在浏览器渲染完成之前执行的，也就是说useLayoutEffect总是比useEffect先执行

  > 绝大部分情况下，先用 useEffect，一般问题不大；如果页面有异常，再直接替换为 useLayoutEffect即可

------

#### 46. 使用useState更新数组时需要注意什么

使用useState初始化了一个状态数组/对象，使用push、pop / object.attribute=values、delete object.attribute等数组/对象修改方法在setState是不会起作用的，应该用结构赋值的方式将数组/对象重新赋值，再setState才能让React获得其值。例如：

```js
const [count, setCount] = useState([0, 1, 2]);
const {num,setNum} = useState({a: 1})
// 不起作用的写法：
// count.push(3);
// setCount(count);
// num.b = 2;
// setNum(num);

// 正确的写法：
count = [...count, 3];
setCount(count);
num = {...num,b: 2};
setNum(num);
```

造成这种现象的原因：

当使用useState初始化一个引用类型的状态时，React会追踪该引用，并在状态更新时比较新旧引用，再决定要不要重新渲染。如果直接在原先的引用类型数据上添加、删除或者修改属性，其原始引用并没有发生改变，React无法察觉到对象的属性发生修改。因此我们需要创建一个新的引用类型状态，常见的方法有解构赋值，深拷贝等，再赋值给之前的引用，确保React能检测到状态的变化

------

#### 47.  React Hooks和生命周期的关系

生命周期属于类组件的概念，函数组件一旦开始渲染就不能中断，因此不存在生命周期的概念。但是React团队提供了一些强大的hooks，让我们在函数组件中也能使用到类似生命周期钩子的功能。

以下是类组件的生命周期和函数组件的hooks对应的关系表：

| 类组件                   | 函数组件                                                     |
| ------------------------ | ------------------------------------------------------------ |
| constructor              | useState中定义的初始化状态                                   |
| getDerivedStateFromProps | useState中定义的更新状态函数                                 |
| shouldComponentUpdate    | useMemo                                                      |
| render                   | 函数本身                                                     |
| componentDidMount        | useEffect的第二个参数填空数组                                |
| componentDidUpdate       | useEffect的第二个参数填入需要监视的状态组成的数组            |
| componentWillUnmount     | useEffect的第一个参数是回调函数，在回调函数内再返回一个清理函数则会触发 |
| componentDidCatch        | 无                                                           |
| getDerivedStateFromError | 无                                                           |

------

#### 48. 对虚拟DOM的理解

虚拟DOM本质上是一个Js对象，通过对象的形式描绘真正的DOM结构，配合不同的渲染工具，使跨平台渲染成为可能。通过事务处理机制，将多次DOM修改的结果一次性的更新到页面上，从而有效的减少页面渲染的次数，减少修改DOM的重绘重排次数，提高渲染性能。

------

#### 49. React的Diff算法的原理与React Fiber架构中算法的变化

React Diff算法的原理是通过递归比较两棵虚拟DOM树的节点，找出他们的差异，并将差异更行到真实DOM树上，这样可以避免整个更新真实DOM树，提高性能。但是美中不足在于递归算法存在不可中断性，如果虚拟DOM树过于庞大会导致页面无法响应。

但是在最新的Reat Fiber架构中，React团队对算法做了一些优化，主要表现在将任务切片和优先级调度上，实际上就是将两棵虚拟DOM树同层的节点拆分成一个个Fiber结构再比较，并动态的调整任务调度的优先级，在浏览器的空闲时间工作，如果有用户交互，则将主线程交还给用户，以提供更好的用户体验。

------

#### 50. React和Vue中的key的作用

在两大框架中，keys都用于追踪哪些元素被修改、被添加或者被移除的辅助标识。在两者的Diff算法中也会借助key值来判断该元素是新创建的还是别处移动来的，从而减少不必要的元素渲染。此外React 还需要借助key值来判断元素与本地状态的关联关系。

------

#### 51. 虚拟DOM与直接操作真实DOM相比，哪一个效率更高

这种需要分情况讨论，如果只是操作一个按钮的文字，通过虚拟DOM修改完全不可能比得过直接操作真实DOM。虚拟DOM相较于真实DOM真正的优越之处在于能够提供给开发者更舒适的开发前提的前提下，还能保持不错的性能。

------

#### 52. React的严格模式

StrictMode是一个用来突出显示应用程序中潜在问题的工具。与 `Fragment` 一样，`StrictMode` 不会渲染任何可见的 UI。它为其后代元素触发额外的检查和警告。 可以为应用程序的任何部分启用严格模式。只需要在用`<React.StrictMode></React.StrictMode>`包裹住想开启严格模式的组件即可。

StrictMode有助于：

- 识别不安全的生命周期
- 关于使用过时字符串 ref API 的警告
- 关于使用废弃的 findDOMNode 方法的警告
- 检测意外的副作用
- 检测过时的 context API

------

#### 53. 在React中遍历的方法

- 遍历数组：map，forEach
- 遍历对象：用`Object.entries()`（需要对象的键值对）/`[...object]`（只需要对象的值）把对象转化成数组再用map遍历，for...in

------

#### 54. 对React SSR和Vue SSR的理解

即由服务端直接完成页面的渲染，再通过网络交由客户端展示。两大框架的基本工作流程如下：

- 服务器接收到客户端的请求
- 服务器创建初始的React/Vue组件树
- 服务器生成HTML
- 服务器将生产的HTML作为响应发送给客户端
- 客户端上的React/Vue组件重新挂载并接管页面交互

SSR的优势：

- 利于SEO优化
- 所有模板、图片等资源都存在服务器端
- 一个html返回所有数据
- 减少HTTP请求
- 响应快、用户体验好、首屏渲染快

SSR的劣势：

- 服务器端压力大
- 开发条件受限，React SSR中只能使用`constructor()`、`getDerivedDefaultProps()`和`render()`三个钩子，Vue SSR中只能使用`beforeCreate()`和`Created()`两个钩子
- 学习成本高，需要开发者具有全栈开发的能力

#### 