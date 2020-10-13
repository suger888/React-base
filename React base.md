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