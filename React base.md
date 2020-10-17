 react 不像一个框架，更像一个生态（很多react家族插件组成的）

 React如何挂载视图？
     1：生成虚拟节点（如何生成虚拟节点）
  		react.createElement("h3",null,["hello react])
     2.挂载虚拟节点转换的真实节点
        如何挂载
         render（挂载的东西，document.getElementById("ROOT")）
 React 中的JSX语法
     首先需要引入一个babel.js文件 这个是解析jsx语法的
     然后在script标签中写入text=“text/babel”
     然后就可以像js中一样写入 let Vnode=<h3> hello React</h3>

react 中怎么写插值表达式
        通过｛｝就可以像Vue中一样 注意点｛｝中可以写任意表达式 但是不能写纯对象 纯函数（函数调用可以写）
         另外｛｝里面写入undefined，null,布尔值 不显示在视图上 


react 中属性绑定怎么写？
        class=｛属性名｝

react 中添加事件
	直接在标签中写事件 列如onClick={}
 react传参
    	原生js中事件传参方法：
	1.写匿名函数，匿名函数内传参
	2.闭包传参
	3.bind传参

react事件句柄内的this默认指向undefined（不会指向组件，更不会指向按钮）

react添加事件 中想要处理事件对象的时候 利用bind来处理


React组件有三种
	1：函数组件
	2.createClass组件
	3.class组件（es6中的类）

	函数组件写法：
		function App(){
			return (
			<div></div>
			)	
			}
	类组件写法：
		class App extends React.component{
				constrcoter(){
					super()	
				this.msg="hello world"
				}
				render(){
		注意点这里的render函数里面的this指向的是类组件实例
		用类组件中的数据 需要用this，不然拿不到
				return (
				<div>{msg}</div>
				<button onClick={this.fn.bind(this,i)}>按钮</button>
				)
				}
				fn(){
		注意点：fn函数里面的this 指向的是undefined 
				consolo.log(66)
				this.arr.splice(i,1)
				}
		}
	注意点：如果要修改类组件中的数据，在button 中利用bind修改this的指向

	注意补充：在react中this.forceUpdata()表示强制刷新视图
		注意在Vue中 this.$forceUpdata() 也可以强制刷新视图 之前是说修改数组元素不能刷新视图  如果用强制刷新视图可以更新视图
	ReactDOM.render(<App/>,document.getElementById("root"))








Vue中的插槽是用来复用模板的

React中也是一样复用组件 并且可以快速获取祖组件的内容 不用一层一层传递下去

		this.props.children
	class Item extend React.Component{
		render(){return(
			<div>
			    <p>111111</p>
			    <div>item组件</div>
			    ｛this.props.children｝
			</div>
			)}
	}

	class App extends React.Component{

			render(){return(
				<div>
				    <item>
					<h3>我是插槽</h3>
				    </item>
				</div>
			)}
		}

这里的<h3>我是插槽</h3> 会显示在Item组件上对应的｛this.props.children位置上




==========================================================

React 父子解耦：断绝父子关系 父组件与子组件之前没有联系
 
<script type='text/babel'>
    
    const {Component,Fragment} = React;

    // 插槽可以复用组件,并且可以快速实现多层传值.

    class Son extends Component{
        render(){
            return (
                <div>                   
                    <h3>Son子组件</h3>
                    <div>{this.props.msg}</div>
                </div>
            )
        }
    }

    class Item extends Component{
        render(){
            console.log(this.props.children)
            return (
                <div>
                    <h3>子组件</h3>
                    <div>11111</div>
                    <hr />
                    {this.props.children}
                </div>
            )
        }
    }

    class App extends Component{
        str = 'App的数据'
        render(){          
            return (
                <Fragment>
                    <Item>
                        <Son msg={this.str} />
                    </Item>
                </Fragment>
            )
        }
    }
注意点：以上有三个组件App是爷爷组件，Item是父组件，Son是孙子组件 通过插槽 可以实现快速把爷爷组件中的数据传递给孙子组件 Son 组件可以快速访问到爷爷组件中的数据，不用一层一层传递了 同时造成了父组件Item与孙子组件Son之前没有联系 也叫父子解耦
    ReactDOM.render(<App />,document.getElementById('root'));

</script>
	另外解决组件之前多层传递的第二种方式利用 React 中的API context （这种方法可以用，但是会让组件的复用性变低，做个了解就好 后期用usecontext）
	contenxt方法实现步骤：
		1.先创建一个上下文对象 const ThemeContext=React.createContext("默认值")  （ThemeContext是变量名）
//注意这里的括号可以不写内容 写了的话 就显示这个值
		2.创建的ThemeContext是一个对象 里面有一个Provider 组件 利用解构方便书写  const {Provider} = ThemeContext;
		3.用组件Provider 包裹住需要访问的组件 包裹住组件内以及所有的子孙组件都可以访问Provider 的value属性。
		4.需要访问的组件内需要写 static contextType =ThemeContext
		5.在该组件内通过this.context来获取最近的Provider 上的value.
		6.注意：如果没有任何Provider 。this.context获取的是createContext()这个括号内写的值

		案列如下：
		<script type='text/babel'>
     const ThemeContext = React.createContext('默认值');
    const {Component,Fragment} = React;

    const {Provider} = ThemeContext;

    class Son extends Component{
        // 静态属性contextType
        static contextType = ThemeContext;
        render(){
            return (
                <div>                   
                    <h3>Son子组件</h3>
                    <div>{this.context}</div>
                </div>
            )
        }
    }

    class Item extends Component{
        render(){
            return (
                <div>
                    <h3>Item子组件</h3>
                    <Provider value='我最近，选我'>
                        <Son />
                    </Provider>
                </div>
            )
        }
    }

    // Provider包裹住的组件以及所有的子孙组件,都可以访问Provider的value属性.
    class App extends Component{
        str = 'App的数据'
        render(){          
            return (
                <Fragment>
                    <Provider value={this.str}>
                        <Item></Item>
                    </Provider>
                </Fragment>
            )
        }
    }

    ReactDOM.render(<App />,document.getElementById('root'));

    // Vue多层父传子的解决方案:
    //    1:一层一层传递
    //    2:插槽传递。(父子解耦,解决办法是作用域插槽)
    //    3:bus传值.

    // React多层父传子的解决方案:
    //    1:一层一层传递
    //    2:插槽传递。(父子解耦,解决办法是renderProps)
    //    3:context。(可用,但是会让组件的复用性变低,后期只需要使用useContext)

    // context -> 用于多层传值。

    // 创建一个上下文对象
    // 通过ThemeContext解构获取Provider组件.
    // Provider包裹住的组件以及所有的子孙组件,都可以访问Provider的value属性.
    // 如何访问?
    // 1:在访问的组件内:static contextType = ThemeContext;
    // 2:在组件内通过this.context来获取最近的Provider上的value.
    // 如果没有任何Provider,this.context获取的是createContext的默认值参数.
==================================================================
解决父子解耦的方法：
	1.利用React中的renderProps方式
		renderPorps原理就是：
 		renderProps方法 实际上就是传一个方法给子组件 然后这个方法中返回一孙子组件  在子组件中接收并且调用这个方法 把子组件中的数据用参数的形式传值给孙子组件  并且在这个孙子组件中传值.
例如如下
<script type='text/babel'>
    
    const {Component,Fragment} = React;

    // 插槽可以复用组件,并且可以快速实现多层传值.

    class Item extends Component{
        msg = 'Item的数据'
        render(){
            let {data,render} = this.props;
            return (
                <div>
                    <h3>Item子组件</h3>
                    <hr />
                    {data}
                    {render && render(this.msg)}
                </div>
            )
        }
    }

    function Box (props){
        return <div>{props && props.msg}</div>
    }

    class App extends Component{
        str = 'App的数据'
        render(){          
            return (
                <Fragment>
                    <Item data='8888' />
                    <Item data={1+1} />
                    <Item data={<h3>propsh3</h3>} />
                    <Item data={<Box />} />
                    <Item render={()=>{return 9999}} />
                    <Item render={(msg)=><Box msg={msg} />} />
                </Fragment>
            )
        }
    }
// renderProps方法 实际上就是传一个方法给子组件 然后这个方法中返回一个孙子组件  在子组件中接收并且调用这个方法 把子组件中的数据用参数的形式传值给孙子组件  并且在这个孙子组件中传值
    ReactDOM.render(<App />,document.getElementById('root'));

</script>

============================================================================


React中的数据更新或者说是修改数据  是利用state=｛｝ React 的数据一般都放在state对象中 如果修改state对象中的数据 需要利用setState（）方法来修改这样才能视图更新 如果直接用this.state.num=2 来修改的话是不会引起视图变化的
	state={
		num:0
	}
	1.fn(){

		this.setState({
		num:this.state.num+2
	})
		console.log(this.state.num) // 此时打印出来的是上次点击时候的数值
	}
	
	这里有一个num 数据 有一个按钮点击一下就this.state.num+2
 	因为this.setState()是异步的，用上面的方法写虽然能实现视图数据更新 不过不稳定 容易出现视图上是上次点击的数字
	
----------------------------------------------------------------------------
注意点：如果state对象中的数据是引用数据类型 不能改变这个数据的引用类型 不能改变原数据 有两个方法 ：
		1.利用缓存 把数据缓存下赋值个给新数组
			例如state=｛
				arr:[1,2,3,4,5]
			｝
		修改state中的arr中的数据
			newArr=this.state.arr.slice() // slice()方法里面不写东西就是原封不动的arr数据 截取的是空
	    然后this.setState（｛arr:newArr.splice(i,1)｝）


		2.利用...扩散符
		arr:[1,2,3,4,5,6]
		 this.setState({
                arr:[
                    ...this.state.arr.slice(0,i),
                    ...this.state.arr.slice(i+1)
                ]

		注意点：此时这里的arr是被截取出来然后利用...扩散符放在一个数组里
            });  

-------------------------------------------------------------------------
	     // React推崇"数据不可变性"
            // 1:不要修改原来的引用类型.
            // 2:数据变化后,应该是一个新的引用,而不是原来的引用(对象).

只要满足以上两个原则 什么写法都可以实现数据更新

this.setState() 在setState() 修改数据的过程是异步的 this.setState({},()=>{})
setState() 括号里有两个参数 第二个是回调函数 这个回调函数在数据修改完成时,自动触发

	React中的state 类似于Vue中的data 
	不同的是Vue中的data 几乎什么数据都可以放在data 里，但是React 中不同
	那么问题来了 什么数据应该是放在state内的数据呢
		1:该数据是否是由父组件通过 props 传递而来的？如果是，那它应该不是 state内的数据。
   		2:该数据是否随时间的推移而保持不变？如果是，那它应该也不是 state的数据。
   		 3:你能否根据其他 state 或 props 计算出该数据的值？如果是，那它也不是 state的数据。
============================================================================

 React中的高阶组件
	什么事高阶组件?
		高阶组件本质就是一个函数 这个函数是用函数封装思维写出来的 他的作用是把组件之间的相同逻辑封装起来.并且用来加工这个组件
	如果有两个组件Item组件 和Son组件

		class Item extends Component{
        render(){
            return (
                <div>                   
                    <p>{this.props.data}</p>
                </div>
            )
        }
    }
		class Son extends Component{
        render(){
            return (
                <div>                   
                    <p>{this.props.data}</p>
                </div>
            )
        }
    }

这两个组件的模板除了p和H3之间不同其他都相同
	现在写一个高阶组件也就是一个函数
	function HOC( Component){
		data=95825
		return <Componnet data={data} />
	}
	let NewItem=HOC(Item)  // NewItem经过HOC高阶组件加工处理后的新组件
	letNewSon=HOC(Son)    // NewSon经过HOC高阶组件加工处理后的新组件

 class App extends Component{
        render(){          
            return (
                <Fragment>
                    {NewItem}
                    {NewSon}
                </Fragment>
            )
        }
    }

    ReactDOM.render(<App />,document.getElementById('root'));
	
============================================================================

React 受控组件 （实现双向数据绑定）
	受控组件是针对于表单元素来讲的 只有表单元素能实现双向数据绑定
	表单元素有：input系列，文本域textarea ，select
	表单元素写了value就是受控组件 没有vlaue就是非受控组件

	受控组件的性质？
	1.value必须绑定state内的数据
	2.受控组件的vlaue 变化，必须用过修改state来更新
	3.修改state内绑定的数据，必须用过onChange 事件来触发
	
	 如下演示
	const {Component,Fragment} = React;
	class App extends Component{

        state = {
            msg:10
        }

        render(){         
            return (
                <Fragment>
                    <input type='text' onChange={this.fn.bind(this)} value={this.state.msg} />
                    <div>总价:{this.state.msg}</div>
                </Fragment>
            )
        }

        fn(ev){           
            this.setState({
                msg:ev.target.value
            })
        }
    }

    ReactDOM.render(<App />,document.getElementById('root'));


	如果是处理表单元素 的复选框 受控组件是这样处理的
	如果受控组件是一个复选框,则应该吧flag绑定到checked属性上

	 const {Component,Fragment} = React;

    

    class App extends Component{

        state = {
            flag:true
        }

        render(){     
            return (
                <Fragment>
                    <input type='checkbox' onChange={this.fn.bind(this)} checked={this.state.flag} />
                    { this.state.flag && <div>11111111111</div> }
                </Fragment>
            )
        }

        fn(ev){
            // 设置total并且更新视图
            this.setState({
                flag:ev.target.checked
            })
        }
    }

    ReactDOM.render(<App />,document.getElementById('root'));


	如果受控组件是一个单选框,应该控制checked属性.但是状态不应该是布尔值
	 
    const {Component,Fragment} = React;
    class App extends Component{

        state = {
            sex:'男'
        }

        render(){     
            return (
                <Fragment>
                    <input 
                        type='radio' 
                        name='sex' 
                        onChange={this.fn.bind(this)} 
                        checked={this.state.sex == '男'}
                        value='男' 
                    />男
                    <input 
                        type='radio' 
                        name='sex' 
                        onChange={this.fn.bind(this)} 
                        checked={this.state.sex == '女'}
                        value='女' 
                    />女
                    <div>性别:{this.state.sex}</div>
                </Fragment>
            )
        }

        fn(ev){
            // 设置sex并且更新视图
            this.setState({
                sex:ev.target.value
            })
        }
    }

    ReactDOM.render(<App />,document.getElementById('root'));
    	============================================================================

React 生命周期

	1.创建阶段
		constructor(类似于vue的created，用于初始化数据)
		componentWillMount ()
		render （更新虚拟节点） 
注意点：render函数里面只能做读的操作 不能做写的操作 不然造成递归 形成死循环 报错             每次视图更新，render会重新调用，返回新的虚拟节点，转换成新的视图，显示新的数据状态
		componentDidMount 用于请求数据（类似于mounted）
	Vue中的父子组件的生命周期执行顺序
		 父beoforeCreate
        	 父created
             	父beoforeMount
            	 子beoforeCreate
            	 子created
            	 子beoforeMount
            	子mounted
            	父mounted
	React中的父子组件的生命周期执行顺序
		父construtor
		父componentWillMount
		父render
		子construtor
		子componentWillMount
		子render
		子componentDidMounted
		父componentDidMounted

	如果要改变生命周期的执行顺序 
		vue中利用在子组件标签中用v-if 
		React中利用表达式 this.flag&&子组件 先是flag=false 等执行完了父componentDidMounted 利用这个函数把flag变成true
============================================================================

React 状态管理  
	1.多个组件可以共享同一个数据
	2.这个数据变化，可以同时更新多个组件
		
React 的状态管理有Redux ,Mobx
	Redux,Mobx不是React 的插件 是第三方的 	


Redux的写法
	Redux的实例化写法推演
	
	利用闭包去存储数据num
    存储一个数据
     function reducer (num=0){
		return num 
	}
	
	const store=Redux.createStore(reducer)

	获取数据num的方式
	store.getState()
  问题来了，我想存储两个数据怎么写
	写两个函数
	function reducer1(num=0){return num}
	function reducer2(msg=100){return msg}
	const store1=Redux.createStore(reducer1)
	const store2=Redux.createStore(reducer2)
	获取数据
	store1.getState()	
	store2.getState()

多个数据怎么办呢 
	以上的两个数据的函数可以合并成一个函数
	let reducer=combineRedecers({reducer1},{reducer2})
	const store=Redux.createStore(reducer)
	const store=Redux.createStore(combineReducers({reducer1,reducer2})

解构赋值 const {createStore,combineReducers}=Redux
	最终Redux的真实实例是下面这个
	const store=createStore(combineRducers({reducer1,reducer2}))
	获取数据就用 store.getState().reducer1
		    store.getState().reducer2

怎么样修改共享数据？
	把修改数据的逻辑方法写在存储数据的函数中 这个函数有两个形参 第一个是数据 第二个是actions 利用type 用判断可以实现想要做什么操作
	然后在组件中写事件 在这个事件中获取这个方法 
	用store.dispatch({
		type:"PLUS",
		step:2
	})

	以上我们都需要强制刷新视图 因为还不是state中的数据 数据变化是不会自动更新视图 
	react-redux插件如何实现响应式视图更新?
            需要共享数据的组件通过一个Provider组件包裹
           Provider组件通过插件react-redux提供
           当数据变化时,Provider会自动更新,被Provider组件包裹的其他组件也会自动更新

        mapState把state数据映射成计算属性computed
        mapMutations把修改数据的方法映射成组件的methods

所以我们需要引入React-Redux插件 插件中提供了Provider组件 用这个组件包含想要用共享数据的组件 然后引起Provider组件的更新九会造成视图更新
	那么怎么样引起Provider组件更新呢
	我们用高阶组件来引发更新 利用connect高阶组件 把现有的组件和redex结合,让redux的数据个方法变成组件的props 
connect的第一个参数是映射state,第二个参数是映射dispatch
如果一个组件只显示共享数据,不修改共享数据,可以没有第二个参数

	案例演示如下：
		<script type="text/babel">

        const {Component,Fragment} = React;
        const {createStore,combineReducers} = Redux;
        const {Provider,connect} = ReactRedux;
        
        //   通过高阶组件connect,把现有的组件和redex结合,让redux的数据个方法变成组件的props
        //   connect的第一个参数是映射state,第二个参数是映射dispatch
        //   如果一个组件只显示共享数据,不修改共享数据,可以没有第二个参数

        function getNum(num=100,actions){           
            switch(actions.type){
                case 'PLUS':
                    num += actions.step;
                    return num
                case 'REDUCE':
                    num -= actions.step;
                    return num
                default:
                    return num
            }
        }

        // 实例化
        const store = createStore(combineReducers({getNum}));

        class Son extends Component{
            render(){
                let {plus,reduce,num} = this.props;
                return (
                    <Fragment>
                        <button onClick={()=>{plus(1)}}>num++</button>
                        <button onClick={()=>{reduce(1)}}>num--</button>
                        <div>{num}</div>
                    </Fragment>   
                )
            }
        }
              
        class Item extends Component{
            render(){
                let {plus,reduce,num} = this.props;
                return (
                    <Fragment>
                        <button onClick={()=>{plus(2)}}>num++</button>
                        <button onClick={()=>{reduce(2)}}>num--</button>
                        <div>{num}</div>
                    </Fragment>   
                )
            }
        }

        // 把state映射成Props
        function mapStateToProps(state){
            // 形参state就是store.getState()
            return {
                // num:store.getState().getNum
                num:state.getNum
            }
        }

        // 把dispatch映射成props
        function mapDispatchToProps(dispatch){
            // 这里的形参dispatch就是Store的dispatch方法
            return {
                plus(step){
                    dispatch({
                        type:'PLUS',
                        step
                    })
                },
                reduce(step){
                    dispatch({
                        type:'REDUCE',
                        step
                    })
                }
            }
        }

        // 高阶组件将Item和redux结合
        const NewItem = connect(mapStateToProps,mapDispatchToProps)(Item);
        // 高阶组件将Son和redux结合
        const NewSon = connect(mapStateToProps,mapDispatchToProps)(Son);

        class App extends Component{
            render(){
                return (
                    <Fragment>
                        <h3>App组件</h3>
                        <div>{this.props.num}</div>
                        <NewItem />
                        <NewSon />
                    </Fragment>   
                )
            }
        }

        // App只显示,不修改,可以不传mapDispatchToProps
        const NewApp = connect(mapStateToProps)(App);

        ReactDOM.render((
            <Provider store={store}>
                <NewApp />
            </Provider>   
        ),document.getElementById('root'))

    </script>


===============================================================================
React 路由4.0以下的版本插件名叫做react-router
React路由5.0+的版本插件名叫做react-router-dom

React路由没有路由配置

	hashRouter包裹包住所有路由内置组件
	NavLink 是a标签
	Route组件就是设置path和component的对应关系 
	Redirect组件用于重定向.
	Switch的作用就是让Route的匹配与Vue的routes一致
        从上往下匹配,匹配上的就不再往下判断.

	// React的path匹配是包含匹配
        //    url的path包含Route内的path,就可以显示组件.
        // 如果想让Route的path匹配变成完全匹配,需要设置exact属性


	  Route设置与path匹配的组件,可以有以下方式:
          1:component = {类/函数}
          2:render= {()=>路由组件}
          3:children= {路由组件}
          4:<Route>路由组件</Route>

	嵌套路由就是在组件中再写一个<HashRouter></HashRouter>

	如下：
		class News extends Component{
			render(){
				return (
                    <Fragment>
                        <h3>News组件</h3>
                        <HashRouter>
                            <NavLink to='/news/'>手机</NavLink>
                            <NavLink to='/news/computer'>电脑</NavLink>
                            <NavLink to='/news/tv'>电视</NavLink>
                            <Switch>
                                <Route path='/news/' exact children={<div>手机</div>} />                        
                                <Route path='/news/computer' children={<div>电脑</div>} />                        
                                <Route path='/news/tv' children={<div>电视</div>} />                                         
                            </Switch>  
                        </HashRouter>
                    </Fragment>
                )
			}
        }

	路由的跳转
	路由的跳转 是利用this.props里面的3个常用数据 history，location，match
	组件是被<Route/>组件赋予这三个值给this.props的 如果没有《Route》包住该组件则访问不到
	如果没有被包住 但是想访问this.props的话 利用高阶组件 withRouter（）
	进行加工
	列如 App组件没有《Route/》包住 则const NewApp=withRouter(App)
	再重新挂载render 上
	
	演示如下：
			<script type="text/babel">
		
        const {Component,Fragment} = React;
        const {HashRouter,Route,NavLink,Redirect,Switch,withRouter} = ReactRouterDOM;
        
        // 路由的各种路由数据(path,params,push)和方法都可以通过props获取
        // props内的3个常用属性:
        //  1:hisory
        //  2:location
        //  3:match

        // 以上3个属性是通过Route组件传递给路由组件Home,News,Sport的
        // 在App内的props获取不到以上三个属性，那是因为App不是Route的子组件。

        // 任何组件想要使用props内的history,location和match属性,都可以通过withRouter来处理

        class Home extends Component{
			render(){
				return <h3>Home组件</h3>
            }
        }
        
        class News extends Component{
			render(){
				return <h3>News组件</h3>
			}
        }
        
        class Sport extends Component{
			render(){
				return <h3>Sport组件</h3>
			}
		}

		class App extends Component{
			render(){
				return (
                    <Fragment>
                        <button onClick={this.toPage.bind(this,'/')}>首页</button>
                        <button onClick={this.toPage.bind(this,'/news')}>新闻</button>
                        <button onClick={this.toPage.bind(this,'/sport')}>体育</button>
                        <Switch>
                            <Route path='/' exact component={Home} />                        
                            <Route path='/news' component={News} />                        
                            <Route path='/sport' component={Sport} />
                        </Switch>                       
                    </Fragment>
				)
            }
            
            toPage(path){
                console.log(this.props);
                this.props.history.push(path);
            }
		}

        // 让App可以使用props的history，location和match。
        const NewApp = withRouter(App);

		ReactDOM.render((
            <HashRouter>
                <NewApp />
            </HashRouter>
        ),document.getElementById('root'));


	</script>

	路由传参
		路由传参是利用this.props里面三个常用数据的history方法 在跳转的时候传到对应的组件中  写法如下：
	this.props.history.push({pathname:'目标地址名'，params:{msg:"数据"})
	接收的时候在对应的地方接收 {this.props.localtion.params.msg}

 注意点：这里的params 不是固定的 也可以自定义名字 也可以不写成对象

	动态路由
	什么是动态路由：动态路由就是路由的组件都是一样的 可以赢一套组件模板 只是路由选项 只是改变路由组件的内容
		路由拦截 Prompt组件 用法如下

		 <script type="text/babel">
                let {Fragment,Component}=React
                let{HashRouter,NavLink,Route,exact,Switch,Redirect,Prompt}=ReactRouterDOM
                class Home extends Component{
                    render(){
                        return(
                            <Fragment>
                                <h3>{this.props.match.params.path}</h3>
                                <Prompt message='你确定要走'/>
                                {/* <Prompt message={()=>{return false}} /> */}
                        {/*<Prompt when={false} message={this.handler.bind(this)} />*/}
                            </Fragment>
                        )
                    }
                    componentWillReceiveProps(nextProps){
                        console.log(nextProps);
                        console.log(this.props);
                    }
                   
                }
              
                class App extends Component{
                    render(){
                        return(
                            <Fragment>
                                <HashRouter>
                                    <NavLink to='/home'>首页</NavLink>
                                    <Route path='/:path' component={Home}/>
                                    <Redirect path='/' exact component={Home}/>
                                </HashRouter>
                            </Fragment>
                        )
                    }
                } 
                ReactDOM.render(<App/>,document.getElementById('root'))
    </script>

	怎么样设置全局配置拦截呢 （能让所有的组件都能购）
	利用高阶组件的特质 给每个组件进行加工，
	利用高阶组件的特质 返回一个匿名类组件 匿名类组件中返回的模板中有		<Prompt/>
	列如：

		<script type="text/babel">
		
        const {Component,Fragment} = React;
        const {HashRouter,Route,NavLink,Redirect,Switch,Prompt} = ReactRouterDOM;
        
        // Prompt组件用于组件拦截.类似于Vue的beforeRouteLeave守卫
        // Prompt是一个抽象组件，不会显示到视图上。
        // Prompt的message属性的值可以是字符串也可以是函数
        // message的函数return false就会跳转失败,return true可以正常跳转


        class Home extends Component{
			render(){
				return (
                    <Fragment>
                        <h3>Home组件</h3>
                    </Fragment>
                )
            }
     }
        
        class News extends Component{
			render(){
				return <h3>News组件</h3>
            }
        }
        
        class Sport extends Component{
			render(){
				return <h3>Sport组件</h3>
			}
        }
        
        // 全局的拦截高阶组件
        function HOCRoute(RouteComponent){
            return class extends Component{
                render(){                   
                    return (
                        <Fragment>
                            <RouteComponent />
                            <Prompt message={this.handler.bind(this)} />
                        </Fragment>
                    )
                }
                handler(location){
                    console.log(this.props.location.pathname)
                    return true               
                }
            }
        }

        const NewHome = HOCRoute(Home);
        const NewNews = HOCRoute(News);
        const NewSport = HOCRoute(Sport);

		class App extends Component{
			render(){
				return (
                    <Fragment>
                        <HashRouter>
                            <NavLink to='/'>首页</NavLink>
                            <NavLink to='/news'>新闻</NavLink>
                            <NavLink to='/sport'>体育</NavLink>
                            <Switch>
                                <Route path='/' exact component={NewHome} />                        
                                <Route path='/news' component={NewNews} />                        
                                <Route path='/sport' component={NewSport} />
                            </Switch>                        
                        </HashRouter>
                    </Fragment>
				)
			}
		}

		ReactDOM.render(<App />,document.getElementById('root'));


	</script>

	登录权限原理 ：
	登录权限指的是有些页面跳转需要登录，有些页面跳转不需要	登录 需要跳转的页面需要进行判断 判断是否需要进行登录 如果需要则判断是否已经登录 没有登录则跳到登录页 登录页成功登录后再跳转到想要跳转的页面不需要就直接跳转

	核心点：利用三目运算符进行判断 声明一个标识符 isLogin=false
	 ｛isLogin?<Login/>（登录页组件标签）:目标页组件标签｝
	可以利用高阶组件进行处理 这样一眼就知道哪些组件需要登录哪些直接跳转

			
		<script type="text/babel">
		
        const {Component,Fragment} = React;
        const {HashRouter,Route,NavLink,Redirect,Switch} = ReactRouterDOM;
        
        // 有些页面需要登录，有些页面不需要登录
        // Home不需要登录，News和Sport需要登录
        // 跳转到Home不需要判断是否需要登录,直接跳转
        // 跳到News和Sport需要判断是否需要登录,如果需要登录,再判断是否已经登录成功了
        // 如果需要登录,直接跳转到登录页,登录页成功后再跳到源目标页面
        // { requireLogin ? <Login /> : <Home /> }

        // 登录状态,默认是true
        let isLogin = true;

        class Home extends Component{
			render(){
				return <h3>Home组件</h3>
			}
        }
        
        class News extends Component{
			render(){
				return <h3>News组件</h3>
			}
        }
        
        class Sport extends Component{
			render(){
				return <h3>Sport组件</h3>
			}
        }
        
        class Login extends Component{
			render(){
				return (
                    <div>
                        <h3>登录</h3>
                        <button onClick={this.toPage.bind(this)}>快递登录</button>
                    </div>
                )
            }
            toPage(){
                // 获取重定向到登录组件前的path.
                let path = this.props.location.params.path;
                // 跳转到目标路由
                this.props.history.push(path);
                // 切换登录状态
                isLogin = false;
            }
		}

        function HOC(){
            return class extends Component{
                render(){
                    let {path,children} = this.props;
                    return (
                        <Route path={path} children={
                            isLogin ? 
                            <Redirect to={{pathname:'/login',params:{path}}} /> 
                            : 
                            children
                        } />
                    )
                }
            }
        }

        const PrivateRoute = HOC();

		class App extends Component{
			render(){
				return (
                    <Fragment>
                        <HashRouter>
                            <NavLink to='/'>首页</NavLink>
                            <NavLink to='/news'>新闻</NavLink>
                            <NavLink to='/sport'>体育</NavLink>
                            <Switch>
                                <Route path='/' exact component={Home} />                        
                                <PrivateRoute path='/news' children={<News />} />                        
                                <PrivateRoute path='/sport' children={<Sport />} />
                                <Route path='/Login' component={Login} />
                            </Switch>                        
                        </HashRouter>
                    </Fragment>
				)
			}
		}

		ReactDOM.render(<App />,document.getElementById('root'));


	</script>

===============================================================================
 React中 函数组件与类组件的区别
	1.函数组件没有this 
	2.函数组件没有生命周期
	3.函数组件没有状态

什么时候用函数组件？
	没有状态的组件，都可以使用函数组件
