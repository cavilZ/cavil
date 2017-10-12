### ReactNative
* ReactDOM.render()
ReactDOM.render 是 React 的最基本方法，用于将模板转为 HTML 语言，并插入指定的 DOM 节点。

* JSX 是一个看起来很像 XML 的 JavaScript 语法扩展

* HTML标签 与 React组件 对比
* React 可以渲染 HTML 标签 (strings) 或 React 组件 (classes)。
要创建 HTML 标签，只需在 JSX 里使用小写字母开头的标签名。
` var myDivElement = <div className="foo" />;
React.render(myDivElement, document.body);` 
要创建 React 组件，只需创建一个大写字母开头的本地变量。
`var MyComponent = React.createClass({/*...*/});
var myElement = <MyComponent someProperty={true} />;
React.render(myElement, document.body);`

* JavaScript 表达式
* 属性表达式
要使用 JavaScript 表达式作为属性值，只需把这个表达式用一对大括号 ({}) 包起来
子节点表达式
JavaScript 表达式可用于描述子结点

* JSX延展属性
不要试图去修改组件的属性
推荐做法：var component = <Component foo={x} bar={y} />;
延展属性（Spread Attributes）

* Component
React 允许将代码封装成组件（component），然后像插入普通 HTML 标签一样，在网页中插入这个组件。

* 组件的属性(props)
this.props.xx的形式获取组件对象的属性

* 遍历对象的属性：this.props.children会返回组件对象的所有属性
			    React.Children.map或React.Children.forEach

* PropTypes
组件的属性可以接受任意值，字符串、对象、函数等等都可以。有时，我们需要一种机制，验证别人使用组件时，提供的参数是否符合要求。
组件类的PropTypes属性，就是用来验证组件实例的属性是否符合要求。

* ref 属性(获取真实的DOM节点)
组件并不是真实的 DOM 节点，而是存在于内存之中的一种数据结构，叫做虚拟 DOM。只有当它插入文档以后，才会变成真实的 DOM 。根据 React 的设计，所有的 DOM 变动，都先在虚拟 DOM 上发生，然后再将实际发生变动的部分，反映在真实 DOM上，这种算法叫做 DOM diff ，它可以极大提高网页的性能表现。

例子：比如用户的输入框内容，虚拟 DOM 是拿不到用户输入的。文本输入框必须有一个 ref 属性，然后 this.refs.[refName] 就会返回这个真实的 DOM 节点。由于 this.refs.[refName] 属性获取的是真实 DOM ，所以必须等到虚拟 DOM 插入文档以后，才能使用这个属性，否则会报错。

心得：ref属性在开发中使用频率很高，使用它你可以获取到任何你想要获取的组件的对象，有个这个对象你就可以灵活地做很多事情，比如：读写对象的变量，甚至调用对象的函数。

* state
每个组件只会根据props 渲染了自己一次，props 是不可变的。
为了实现交互，可以使用组件的 state 
当 state 更新之后，组件就会重新渲染自己
render() 方法依赖于 this.props 和 this.state ，框架会确保渲染出来的 UI 界面总是与输入（ this.props 和 this.state ）保持一致

* 初始化state
通过getInitialState() 方法初始化state，在组件的生命周期中仅执行一次，用于设置组件的初始化 state

* 更新 state
通过this.setState()方法来更新state，调用该方法后，React会重新渲染相关的UI。

心得：由于 this.props 和 this.state 都用于描述组件的特性，可能会产生混淆。一个简单的区分方法是，
	this.props 表示那些一旦定义，就不再改变的特性，而 this.state 是会随着用户互动而产生变化的特性。

* 组件的详细说明
render
render() 函数应该是纯粹的，也就是说该函数不修改组件的 state，每次调用都返回相同的结果，不读写 DOM 信息，也不和浏览器交互
心得：不要在render()函数中做复杂的操作，更不要进行网络请求，数据库读写，I/O等操作。

getInitialState
object getInitialState() 初始化组件状态在组件挂载之前调用一次。返回值将会作为 this.state 的初始值。
心得：通常在该方法中对组件的状态进行初始化。

getDefaultProps
心得：该方法在你封装一个自定义组件的时候经常用到，通常用于为组件初始化默认属性。

PropTypes
心得：在封装组件时，对组件的属性通常会有类型限制，如：组件的背景图片，需要Image.propTypes.source类型，propTypes便可以帮你完成你需要的属性类型的检查。

* 组件的生命周期(Component Lifecycle)   类似控制器
组件的生命周期分成三个状态：    
		Mounting：已插入真实 DOM
		Updating：正在被重新渲染
		Unmounting：已移出真实 DOM

心得：你会发现这些React 中组件(Component)的生命周期方法从写法上和iOS中UIViewController的生命周期方法很像，React 为每个状态都提供了两种处理函数，will 函数在进入状态之前调用，did 函数在进入状态之后调用。

* Mounting(装载)
	getInitialState(): 在组件挂载之前调用一次。返回值将会作为 this.state 的初始值。
	componentWillMount()：服务器端和客户端都只调用一次，在初始化渲染执行之前立刻调用。
	componentDidMount()：在初始化渲染执行之后立刻调用一次，仅客户端有效（服务器端不会调用）。

Updating (更新)