# Vue学习笔记

## `Vue`核心

### `Vue` 简介：



#### `Vue`是什么： 一套用于 ==构建用户界面==的==渐进式==`JavaScript`框架

> 渐进式： 
>
> ​		Vue可以自底向上逐层的应用
>
> ​	简单应用：只需要一个轻量的核心库
>
> ​	复杂应用：金额可以引入各式各样的Vue插件

####`Vue` 特点

+ 采用==组件化==模式，提高代码服用率、且让代码更很好维护。

+ ==申明式==编码，让编码人员无需直接操作DOM，提搞开发效率。体积小，编码简洁，运动效率高，适合移动/PC端开发

+ 遵循==MVVM==模式

  

#### 与其他JS框架的关联

1. 借鉴`Angulat`的==模板==和==数据绑定==技术
2. 借鉴`React`的==组件化==和==虚拟DOM==技术

#### `Vue`周边库

1. Vue-cli :Vue脚手架
2. vue-resource 
3. axios 
4. vue-router: 路由 
5. vuex: 状态管理 
6. element-ui: 基于 vue 的 UI 组件库(PC 端)

### 初始`Vue`

![image-20220805211725339](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220805211725339.png)

### 模板语法

#### 效果

![image-20220805213034416](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220805213034416.png)

#### 模板理解

+ 插值语法：
  + 功能：用于解析标签体内容 ![image-20220805213435652](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220805213435652.png)
  + 写法：{{xxx}},xxx是`js`表达式，且可以直接读取到`data`中的所有属性。
+  指令语法：
  + 功能：用于解析标签（包括 标签属性、标签体内容、绑定事件....）
  + 举例：`v-bind:` 可简写为 `:`![image-20220805214107899](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220805214107899.png)
  + 备注：`Vue`中有很多的指令，且形式都是：`V-xxx`,为此我们只是拿 `v-bind`举例。

### 数据绑定

1. 效果：![image-20220807160735332](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220807160735332.png)
2. 单向绑定（`v-bind`）：数据只能从`data`流向页面
3. 双向绑定（`v-model`)：数据不仅能从`data`流向页面，还可以从页面流向`data`。

**注意**：

+ 双向绑定一般都应用在表单类元素上（如:`input`、`select`等）
+ 2.`v-model:value`可以简写为`v-model`，因为`v-model`默认收集的就是`value`值
+ ![image-20220807162130572](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220807162130572.png)

### `el`与`data`的两种写法

+ el的两种写法

  1. ```javascript
     new Vue({
             el: '#root',   //创建时直接配置
             data: {
                 name: 'World',
                 school: {
                     name: '百度',
                     url: 'https://www.baidu.com'
                 }
             }
         })
     ```

  2. ```javascript
      const el = new Vue({
             data: {
                 name: 'World',
                 school: {
                     name: '百度',
                     url: 'https://www.baidu.com'
                 }
             }
         })
         el.$mount('#root')  //通过vue.$mount('#root')指定el的值
     ```

+ data的两种写法

  1. ```javascript
     // 对象式 
     new Vue({
             el: '#root',
             data: {
                 name: 'World',
                 school: {
                     name: '百度',
                     url: 'https://www.baidu.com'
                 }
             }
         })
     ```

  2. ```javascript
     //  函数式
     new Vue({
             el: '#root',
             data() {
                 return {
                     name: 'World',
                     school: {
                         name: '百度',
                         url: 'https://www.baidu.com'
                     }
                 }
             }
         })
     ```

  3. 如何选择：目前对象式和函数式都可以，之后学习到组件时，data必须使用函数式，否则会报错

+ ==注意==

  + 由`Vue`管理的函数，一定不要写箭头函数，一旦写了箭头函数，this就不再是Vue的实例了。

### `MVVM`模型

1. M:模型(Model)：对应data中的数据
2. v:视图(View):模板
3. VM:视图模型(ViewModel):VUe实例对象



### 数据代理

#### 回顾Object.defineProperty方法

```javascript
Object.defineProperty(person, 'age', {
            /* 
               enumerable:true, 控制属性是否可以枚举 默认值为false
               writable:true,  控制属性是否可以被修改 默认值为false
               configurable:true 控制属性是否可以被删除 默认值式为false
            */

            get() {
                console.log('有人读取age属性了')
                return number
            }
            ,
            set(value) {
                console.log('有人修改age属性了,且值式',value)
                number = value
            }
```

#### 何为数据代理

定义：通过一个对象代理对另一个对象中的属性的操作

```javascript
let obj = { x: 100 }
let obj2 = { y: 200 }

Object.defineProperty(obj2, 'x', {
    get() {
        return obj.x
    }
    ,
    set(value) {
        obj.x = value
    }
})
```

#### Vue中的数据代理

1. Vue 中的数据代理：
   + 通过VM对象来代理data对象中的属性操作(读/写)
2. Vue中数据代理的好处：
   + 更加方便的操作data中的数据
3. 基本原理：
   + 通过Object.defineProperty()把data对象中所有属性添加到VM上。
   + 为每一个添加到VM上的属性，都指定一个geeter/setter 。
   + 在getter/setter内部去操作(读/写)data中对应的属性。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript" src="../js/vue.js"></script>

</head>

<body>
    <div id="root">
        <h1>学校名称：{{name}}</h1>
        <h1>学校名地址：{{address}}</h1>
    </div>
</body>

<script type="text/javascript">
    Vue.config.productionTip = false
    const vm = new Vue({
        el: '#root', //el 用于指定当前Vue 实例为那个容器服务 之通常为css选择器字符串。
        data: {
            name: '岳阳职业技术学院',
            address: '岳阳市'
        }

    })

</script>

</html>
```



![image-20220810145801462](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220810145801462.png)

### 事件处理

#### 事件的基本使用：

1. 使用v-on:xxx或@xxx绑定事件，其中xxx是时间名
2. 事件的回调需要配置在methods对象中，最终会在VM上
3. methods中配置的函数，不要用箭头函数！否则this就不是vm了
4. methods中配置的函数，都是被Vue所管理的函数，this的指向是VM或组件实例对象
5. @click='demo' 和@click='demo($event)'效果一致，但后者可以传参

```html
        <body>
		<!-- 准备好一个容器-->
		<div id="root">
			<h2>欢迎来到{{name}}学习</h2>
			<!-- <button v-on:click="showInfo">点我提示信息</button> -->
			<button @click="showInfo1">点我提示信息1（不传参）</button>
			<button @click="showInfo2($event,66)">点我提示信息2（传参）</button>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		const vm = new Vue({
			el:'#root',
			data:{
				name:'尚硅谷',
			},
			methods:{
				showInfo1(event){
					// console.log(event.target.innerText)
					// console.log(this) //此处的this是vm
					alert('同学你好！')
				},
				showInfo2(event,number){
					console.log(event,number)
					// console.log(event.target.innerText)
					// console.log(this) //此处的this是vm
					alert('同学你好！！')
				}
			}
		})
	</script>

```

#### 事件修饰符

Vue中的事件修饰符

1. prevent：阻止默认事件(常用)
2. stop:阻止事件冒泡
3. once：事件只触发一次
4. capture：使用事件的捕获模式
5. self：只有event.target是当前的操作元素是才触发事件
6. passive：事件的默认行为立即执行，无需等待事件回调执行完毕

```html
       ...
       <head>
		<meta charset="UTF-8" />
		<title>事件修饰符</title>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
		<style>
			*{
				margin-top: 20px;
			}
			.demo1{
				height: 50px;
				background-color: skyblue;
			}
			.box1{
				padding: 5px;
				background-color: skyblue;
			}
			.box2{
				padding: 5px;
				background-color: orange;
			}
			.list{
				width: 200px;
				height: 200px;
				background-color: peru;
				overflow: auto;
			}
			li{
				height: 100px;
			}
		</style>
	</head>
	<body>
		
		<!-- 准备好一个容器-->
		<div id="root">
			<h2>欢迎来到{{name}}学习</h2>
			<!-- 阻止默认事件（常用） -->
			<a href="http://www.atguigu.com" @click.prevent="showInfo">点我提示信息</a>

			<!-- 阻止事件冒泡（常用） -->
			<div class="demo1" @click="showInfo">
				<button @click.stop="showInfo">点我提示信息</button>
			<!-- 修饰符可以连续写 -->
			<!-- <a href="http://www.atguigu.com" @click.prevent.stop="showInfo">点我提示信息</a> -->
			</div>

			<!-- 事件只触发一次（常用） -->
			<button @click.once="showInfo">点我提示信息</button>

			<!-- 使用事件的捕获模式 -->
			<div class="box1" @click.capture="showMsg(1)">
				div1
				<div class="box2" @click="showMsg(2)">
					div2
				</div>
			</div>

			<!-- 只有event.target是当前操作的元素时才触发事件； -->
			<div class="demo1" @click.self="showInfo">
				<button @click="showInfo">点我提示信息</button>
			</div>

			<!-- 事件的默认行为立即执行，无需等待事件回调执行完毕； -->
			<ul @wheel.passive="demo" class="list">
				<li>1</li>
				<li>2</li>
				<li>3</li>
				<li>4</li>
			</ul>

		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		new Vue({
			el:'#root',
			data:{
				name:'尚硅谷'
			},
			methods:{
				showInfo(e){
					alert('同学你好！')
					// console.log(e.target)
				},
				showMsg(msg){
					console.log(msg)
				},
				demo(){
					for (let i = 0; i < 100000; i++) {
						console.log('#')
					}
					console.log('累坏了')
				}
			}
		})
	</script>
        ...

```

#### 键盘事件

1. Vue中常用的按键别名：

   + 回车 => enter
   + 删除 => delete(捕获“删除”和“退格键”)
   + 退出 => esc
   + 空格 => space
   + 换行 => tab（特殊，必须配合keydown去使用）
   + 上 =>up
   + 下 =>down
   + 左 =>left

   + 右 => right

2. Vue为提供别名的按键，可以使用原始的key值去绑定，但注意要转为kebab-case（短横线命名）

3. 系统修饰键（用法特殊） ：ctrl alt shift meta

   1. 配合keyup使用：按下修饰键的同时在按下其他键，随后释放其他键，事件才能触发
   2. 配合heydown使用：正常触发事件

4. 也可以使用keyCode去指定具体的按键（不推荐）

5. Vue.config.keyCodes自定义键名 = 键码 可以定制按键名

```html 
        ...
        <body>
		<!-- 准备好一个容器-->
		<div id="root">
			<h2>欢迎来到{{name}}学习</h2>
			<input type="text" placeholder="按下回车提示输入" @keydown.huiche="showInfo">
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
		Vue.config.keyCodes.huiche = 13 //定义了一个别名按键

		new Vue({
			el:'#root',
			data:{
				name:'尚硅谷'
			},
			methods: {
				showInfo(e){
					// console.log(e.key,e.keyCode)
					console.log(e.target.value)
				}
			},
		})
	</script>
        ...
```

### 计算属性 

1. 属性: 要用的属性不存在，要通过已有属性计算得来。
2. 原理: 底层借助了Object.defineproperty方法提供的getter 和setter。
3. get函数什么时候会执行？
   1. 初次读取时会执行一次
   2. 当依赖的数据发生改变时会被再次调用
4. 优势: 与methods实现相比，内部有缓存机制(复用),效率更高，调式方便。
5. 注意:
   1. 计算属性最终会出现在VM中，直接读取使用即可.
   2. 如果计算属性要被修改,那必须写set函数去相应修改，且set中要引起计算时依赖的数据发生改变

```html
        ...
        <body>
		<!-- 准备好一个容器-->
		<div id="root">
			姓：<input type="text" v-model="firstName"> <br/><br/>
			名：<input type="text" v-model="lastName"> <br/><br/>
			测试：<input type="text" v-model="x"> <br/><br/>
			全名：<span>{{fullName}}</span> <br/><br/>
			<!-- 全名：<span>{{fullName}}</span> <br/><br/>
			全名：<span>{{fullName}}</span> <br/><br/>
			全名：<span>{{fullName}}</span> -->
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		const vm = new Vue({
			el:'#root',
			data:{
				firstName:'张',
				lastName:'三',
				x:'你好'
			},
			methods: {
				demo(){
					
				}
			},
			computed:{
				fullName:{
					//get有什么作用？当有人读取fullName时，get就会被调用，且返回值就作为fullName的值
					//get什么时候调用？1.初次读取fullName时。2.所依赖的数据发生变化时。
					get(){
						console.log('get被调用了')
						// console.log(this) //此处的this是vm
						return this.firstName + '-' + this.lastName
					},
					//set什么时候调用? 当fullName被修改时。
					set(value){
						console.log('set',value)
						const arr = value.split('-')
						this.firstName = arr[0]
						this.lastName = arr[1]
					}
				}
			}
		})
	</script>
        ...

```

#### 简写

```html
        ...
        <script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		const vm = new Vue({
			el:'#root',
			data:{
				firstName:'张',
				lastName:'三',
			},
			computed:{
				//完整写法
				/* fullName:{
					get(){
						console.log('get被调用了')
						return this.firstName + '-' + this.lastName
					},
					set(value){
						console.log('set',value)
						const arr = value.split('-')
						this.firstName = arr[0]
						this.lastName = arr[1]
					}
				} */
				//简写
				fullName(){
					console.log('get被调用了')
					return this.firstName + '-' + this.lastName
				}
			}
		})
	</script>
        ...

```

### 监视属性

#### 监视属性`watch`

1. 当被监视的属性发生变化是，回调函数自动调用，进行相关操作
2. 监视的属性必须存在，才能进行监视!!!
3. 监视的两种写法：
   1. New Vue 时传入watch配置
   2. 通过VM.$watch监视

```html
        ...
        <body>
		<!-- 准备好一个容器-->
		<div id="root">
			<h2>今天天气很{{info}}</h2>
			<button @click="changeWeather">切换天气</button>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
		
		const vm = new Vue({
			el:'#root',
			data:{
				isHot:true,
			},
			computed:{
				info(){
					return this.isHot ? '炎热' : '凉爽'
				}
			},
			methods: {
				changeWeather(){
					this.isHot = !this.isHot
				}
			},
			/* watch:{
				isHot:{
					immediate:true, //初始化时让handler调用一下
					//handler什么时候调用？当isHot发生改变时。
					handler(newValue,oldValue){
						console.log('isHot被修改了',newValue,oldValue)
					}
				}
			} */
		})

		vm.$watch('isHot',{
			immediate:true, //初始化时让handler调用一下
			//handler什么时候调用？当isHot发生改变时。
			handler(newValue,oldValue){
				console.log('isHot被修改了',newValue,oldValue)
			}
		})
	</script>
        ...

```

#### 深度监视

1. Veu中的Watch默认不监测对象内部值的改变(一层)。
2. 配置Deep:true可以检测对象内部值改变(多层)。

注意：

1. Vue自身可以监测对象内部值的改变，但Vue提供的Watch默认不可以！
2. 使用Watch时根据数据的具体结构，决定是否采用深度监视。

```html
          ...
         <body>
		<!-- 准备好一个容器-->
		<div id="root">
			姓：<input type="text" v-model="firstName"> <br/><br/>
			名：<input type="text" v-model="lastName"> <br/><br/>
			全名：<span>{{fullName}}</span> <br/><br/>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		const vm = new Vue({
			el:'#root',
			data:{
				firstName:'张',
				lastName:'三',
				fullName:'张-三'
			},
			watch:{
				firstName(val){
					setTimeout(()=>{
						console.log(this)
						this.fullName = val + '-' + this.lastName
					},1000);
				},
				lastName(val){
					this.fullName = this.firstName + '-' + val
				}
			}
		})
	</script>
         ...

```

#### 监视属性简写

```html
        ...
        <body>
		<!-- 准备好一个容器-->
		<div id="root">
			<h2>今天天气很{{info}}</h2>
			<button @click="changeWeather">切换天气</button>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
		
		const vm = new Vue({
			el:'#root',
			data:{
				isHot:true,
			},
			computed:{
				info(){
					return this.isHot ? '炎热' : '凉爽'
				}
			},
			methods: {
				changeWeather(){
					this.isHot = !this.isHot
				}
			},
			watch:{
				//正常写法
				/* isHot:{
					// immediate:true, //初始化时让handler调用一下
					// deep:true,//深度监视
					handler(newValue,oldValue){
						console.log('isHot被修改了',newValue,oldValue)
					}
				}, */
				//简写
				/* isHot(newValue,oldValue){
					console.log('isHot被修改了',newValue,oldValue,this)
				} */
			}
		})

		//正常写法
		/* vm.$watch('isHot',{
			immediate:true, //初始化时让handler调用一下
			deep:true,//深度监视
			handler(newValue,oldValue){
				console.log('isHot被修改了',newValue,oldValue)
			}
		}) */

		//简写
		/* vm.$watch('isHot',(newValue,oldValue)=>{
			console.log('isHot被修改了',newValue,oldValue,this)
		}) */

	</script>
         ...

```

#### computed和watch之间的区别：

1. computed能完成的功能，watch都可以完成
2. watch能完成的功能，computed不一定能完成，例如:watch可以进行异步操作。

注意：

1. 所有被Vue管理的函数，最好写成普通函数，这样this的指向才是Vue或组件实例对象。
2. 所有不被Vue管理的函数(定时器的回调、ajax的回调函数、Promise的回调函数)，最好写成箭头函数，这样this的指向才是Vue 或组件实例对象。

```html
          ...
         <body>
		<!-- 准备好一个容器-->
		<div id="root">
			姓：<input type="text" v-model="firstName"> <br/><br/>
			名：<input type="text" v-model="lastName"> <br/><br/>
			全名：<span>{{fullName}}</span> <br/><br/>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		const vm = new Vue({
			el:'#root',
			data:{
				firstName:'张',
				lastName:'三',
				fullName:'张-三'
			},
			watch:{
				firstName(val){
					setTimeout(()=>{
						console.log(this)
						this.fullName = val + '-' + this.lastName
					},1000);
				},
				lastName(val){
					this.fullName = this.firstName + '-' + val
				}
			}
		})
	</script>
         ...

```

### 样式渲染

绑定样式：

1. class样式

   1. 写法 class="xxx" xxx 可以是字符串，对象，数组。
   2. 字符串写法适用于：类名不确定，要动态获取。
   3. 对象写法适用于：要绑定多个样式，个数不确定，名字也不确定。
   4. 数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用。
   
2. stly样式
  
  1. :style="{fontSize: xxx}"其中xxx是动态值。
  2. :style="[a,b]"其中a、b是样式对象。

  
  
  

```html
        ...
        <head>
		<meta charset="UTF-8" />
		<title>绑定样式</title>
		<style>
			.basic{
				width: 400px;
				height: 100px;
				border: 1px solid black;
			}
			
			.happy{
				border: 4px solid red;;
				background-color: rgba(255, 255, 0, 0.644);
				background: linear-gradient(30deg,yellow,pink,orange,yellow);
			}
			.sad{
				border: 4px dashed rgb(2, 197, 2);
				background-color: gray;
			}
			.normal{
				background-color: skyblue;
			}

			.atguigu1{
				background-color: yellowgreen;
			}
			.atguigu2{
				font-size: 30px;
				text-shadow:2px 2px 10px red;
			}
			.atguigu3{
				border-radius: 20px;
			}
		</style>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 
			
		-->
		<!-- 准备好一个容器-->
		<div id="root">
			<!-- 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定 -->
			<div class="basic" :class="mood" @click="changeMood">{{name}}</div> <br/><br/>

			<!-- 绑定class样式--数组写法，适用于：要绑定的样式个数不确定、名字也不确定 -->
			<div class="basic" :class="classArr">{{name}}</div> <br/><br/>

			<!-- 绑定class样式--对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用 -->
			<div class="basic" :class="classObj">{{name}}</div> <br/><br/>

			<!-- 绑定style样式--对象写法 -->
			<div class="basic" :style="styleObj">{{name}}</div> <br/><br/>
			<!-- 绑定style样式--数组写法 -->
			<div class="basic" :style="styleArr">{{name}}</div>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false
		
		const vm = new Vue({
			el:'#root',
			data:{
				name:'尚硅谷',
				mood:'normal',
				classArr:['atguigu1','atguigu2','atguigu3'],
				classObj:{
					atguigu1:false,
					atguigu2:false,
				},
				styleObj:{
					fontSize: '40px',
					color:'red',
				},
				styleObj2:{
					backgroundColor:'orange'
				},
				styleArr:[
					{
						fontSize: '40px',
						color:'blue',
					},
					{
						backgroundColor:'gray'
					}
				]
			},
			methods: {
				changeMood(){
					const arr = ['happy','sad','normal']
					const index = Math.floor(Math.random()*3)
					this.mood = arr[index]
				}
			},
		})
	</script>
         ...

```

### 条件渲染

条件渲染

1.v-if

​	写法：

1. v-if="表达式"
2. v-else-if="表达式"
3. v-else="表达式"

适用于：切换频率较低的场景。

特点：不展示的DOM元素直接被移除。

注意：v-if可以和v-else-if、v-else 一起使用，当要结构不能被"打断"。

2.v-show

写法：v-show="表达式"

适用于：切换评率较高的场景。

特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉

3.备注：使用v-if的时，元素可能无法获取到，而是用v-show一定可以获取到。

```html
	  ...
         <body>		
		<!-- 准备好一个容器-->
		<div id="root">
			<h2>当前的n值是:{{n}}</h2>
			<button @click="n++">点我n+1</button>
			<!-- 使用v-show做条件渲染 -->
			<!-- <h2 v-show="false">欢迎来到{{name}}</h2> -->
			<!-- <h2 v-show="1 === 1">欢迎来到{{name}}</h2> -->

			<!-- 使用v-if做条件渲染 -->
			<!-- <h2 v-if="false">欢迎来到{{name}}</h2> -->
			<!-- <h2 v-if="1 === 1">欢迎来到{{name}}</h2> -->

			<!-- v-else和v-else-if -->
			<!-- <div v-if="n === 1">Angular</div>
			<div v-else-if="n === 2">React</div>
			<div v-else-if="n === 3">Vue</div>
			<div v-else>哈哈</div> -->

			<!-- v-if与template的配合使用 -->
			<template v-if="n === 1">
				<h2>你好</h2>
				<h2>尚硅谷</h2>
				<h2>北京</h2>
			</template>

		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false

		const vm = new Vue({
			el:'#root',
			data:{
				name:'尚硅谷',
				n:0
			}
		})
	</script>
        ...

```

### 列表渲染

#### v-for指令：

1. 用于展示列表数据
2. 语法：v-for="(item,index) in xxx" key="yyy"
3. 可遍历：数组、对象、字符串(用的很少)、指定次数(用的很少)

```html
   <body>
		<!-- 准备好一个容器-->
		<div id="root">
			<!-- 遍历数组 -->
			<h2>人员列表（遍历数组）</h2>
			<ul>
				<li v-for="(p,index) of persons" :key="index">
					{{p.name}}-{{p.age}}
				</li>
			</ul>

			<!-- 遍历对象 -->
			<h2>汽车信息（遍历对象）</h2>
			<ul>
				<li v-for="(value,k) of car" :key="k">
					{{k}}-{{value}}
				</li>
			</ul>

			<!-- 遍历字符串 -->
			<h2>测试遍历字符串（用得少）</h2>
			<ul>
				<li v-for="(char,index) of str" :key="index">
					{{char}}-{{index}}
				</li>
			</ul>
			
			<!-- 遍历指定次数 -->
			<h2>测试遍历指定次数（用得少）</h2>
			<ul>
				<li v-for="(number,index) of 5" :key="index">
					{{index}}-{{number}}
				</li>
			</ul>
		</div>
    </body>

		<script type="text/javascript">
			Vue.config.productionTip = false
			
			new Vue({
				el:'#root',
				data:{
					persons:[
						{id:'001',name:'张三',age:18},
						{id:'002',name:'李四',age:19},
						{id:'003',name:'王五',age:20}
					],
					car:{
						name:'奥迪A8',
						price:'70万',
						color:'黑色'
					},
					str:'hello'
				}
			})
		</script>
                ...

```

#### key作用与原理

面试题：react、vue中的key有什么作用？（key的内部原理）

1. 虚拟DOM中key的作用：
   key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】,
   随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：

2. 对比规则：
   (1).旧虚拟DOM中找到了与新虚拟DOM相同的key：
   ①.若虚拟DOM中内容没变, 直接使用之前的真实DOM！
   ②.若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM。

(2).旧虚拟DOM中未找到与新虚拟DOM相同的key
创建新的真实DOM，随后渲染到到页面。

1. **用**index作为key可能会引发的问题：
2. 若对数据进行：逆序添加、逆序删除等破坏顺序操作:
   会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。
3. 如果结构中还包含输入类的DOM：
   会产生错误DOM更新 ==> 界面有问题。
4. 开发中如何选择key?:
   1.最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值。
   2.如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，
   使用index作为key是没有问题的。

```html
...
         <body>
		<!-- 准备好一个容器-->
		<div id="root">
			<!-- 遍历数组 -->
			<h2>人员列表（遍历数组）</h2>
			<button @click.once="add">添加一个老刘</button>
			<ul>
				<li v-for="(p,index) of persons" :key="index">
					{{p.name}}-{{p.age}}
					<input type="text">
				</li>
			</ul>
		</div>
          </body>
		<script type="text/javascript">
			Vue.config.productionTip = false
			
			new Vue({
				el:'#root',
				data:{
					persons:[
						{id:'001',name:'张三',age:18},
						{id:'002',name:'李四',age:19},
						{id:'003',name:'王五',age:20}
					]
				},
				methods: {
					add(){
						const p = {id:'004',name:'老刘',age:40}
						this.persons.unshift(p)
					}
				},
			})
		</script>
                ...
```

#### 列表过滤

```html
...
<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<h2>人员列表</h2>
			<input type="text" placeholder="请输入名字" v-model="keyWord">
			<ul>
				<li v-for="(p,index) of filPerons" :key="index">
					{{p.name}}-{{p.age}}-{{p.sex}}
				</li>
			</ul>
		</div>

		<script type="text/javascript">
			Vue.config.productionTip = false
			
			//用watch实现
			//#region 
			/* new Vue({
				el:'#root',
				data:{
					keyWord:'',
					persons:[
						{id:'001',name:'马冬梅',age:19,sex:'女'},
						{id:'002',name:'周冬雨',age:20,sex:'女'},
						{id:'003',name:'周杰伦',age:21,sex:'男'},
						{id:'004',name:'温兆伦',age:22,sex:'男'}
					],
					filPerons:[]
				},
				watch:{
					keyWord:{
						immediate:true,
						handler(val){
							this.filPerons = this.persons.filter((p)=>{
								return p.name.indexOf(val) !== -1
							})
						}
					}
				}
			}) */
			//#endregion
			
			//用computed实现
			new Vue({
				el:'#root',
				data:{
					keyWord:'',
					persons:[
						{id:'001',name:'马冬梅',age:19,sex:'女'},
						{id:'002',name:'周冬雨',age:20,sex:'女'},
						{id:'003',name:'周杰伦',age:21,sex:'男'},
						{id:'004',name:'温兆伦',age:22,sex:'男'}
					]
				},
				computed:{
					filPerons(){
						return this.persons.filter((p)=>{
							return p.name.indexOf(this.keyWord) !== -1
						})
					}
				}
			}) 
		</script>
                  ...
```

#### 列表排序

```html
...
<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<h2>人员列表</h2>
			<input type="text" placeholder="请输入名字" v-model="keyWord">
			<button @click="sortType = 2">年龄升序</button>
			<button @click="sortType = 1">年龄降序</button>
			<button @click="sortType = 0">原顺序</button>
			<ul>
				<li v-for="(p,index) of filPerons" :key="p.id">
					{{p.name}}-{{p.age}}-{{p.sex}}
					<input type="text">
				</li>
			</ul>
		</div>

		<script type="text/javascript">
			Vue.config.productionTip = false
			
			new Vue({
				el:'#root',
				data:{
					keyWord:'',
					sortType:0, //0原顺序 1降序 2升序
					persons:[
						{id:'001',name:'马冬梅',age:30,sex:'女'},
						{id:'002',name:'周冬雨',age:31,sex:'女'},
						{id:'003',name:'周杰伦',age:18,sex:'男'},
						{id:'004',name:'温兆伦',age:19,sex:'男'}
					]
				},
				computed:{
					filPerons(){
						const arr = this.persons.filter((p)=>{
							return p.name.indexOf(this.keyWord) !== -1
						})
						//判断一下是否需要排序
						if(this.sortType){
							arr.sort((p1,p2)=>{
								return this.sortType === 1 ? p2.age-p1.age : p1.age-p2.age
							})
						}
						return arr
					}
				}
			}) 

		</script>
                 ...
```

#### Vue监视数据

Vue监视数据的原理：

1. Vue会监视data中所有层次的数据。

2. 如何检测对象中的数据？ 通过setter实现监视，且要在new Vue是传入要检测的数据
  1. 对象中后追加的属性,Vue默认不做响应式处理
  2. 如需给后添加的属性做响应式，请使用如下API：
      1. Vue.set(target,propertyName/index,value)
      2. vm.$set(target,propertyName/index,value)
	3. 如何监测数组中的数据？
	    + 通过包裹数组更行元素的方法实现，本质就是做了两件事:
	      + 调用原生对应的方法对数组进行更新。
	      + 重新解析模板，进而更新页面。
	4. 在Vue修改数组中的某个元素一定要用如下方法:
	    1. 使用这些API：push()、pop()、shift()、unshift()、splice()、sort()、reverse()
	    2. Vue.set()或vm.$set()
	
	==注意==：Vue.set()和vm.$set()不能给vm或vm的根数据对象添加属性!!!
	
	```html
	       ...
	       <body>
			<!-- 准备好一个容器-->
			<div id="root">
				<h1>学生信息</h1>
				<button @click="student.age++">年龄+1岁</button> <br/>
				<button @click="addSex">添加性别属性，默认值：男</button> <br/>
				<button @click="student.sex = '未知' ">修改性别</button> <br/>
				<button @click="addFriend">在列表首位添加一个朋友</button> <br/>
				<button @click="updateFirstFriendName">修改第一个朋友的名字为：张三</button> <br/>
				<button @click="addHobby">添加一个爱好</button> <br/>
				<button @click="updateHobby">修改第一个爱好为：开车</button> <br/>
				<button @click="removeSmoke">过滤掉爱好中的抽烟</button> <br/>
				<h3>姓名：{{student.name}}</h3>
				<h3>年龄：{{student.age}}</h3>
				<h3 v-if="student.sex">性别：{{student.sex}}</h3>
				<h3>爱好：</h3>
				<ul>
					<li v-for="(h,index) in student.hobby" :key="index">
						{{h}}
					</li>
				</ul>
				<h3>朋友们：</h3>
				<ul>
					<li v-for="(f,index) in student.friends" :key="index">
						{{f.name}}--{{f.age}}
					</li>
				</ul>
			</div>
		</body>
	
		<script type="text/javascript">
			Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
	
			const vm = new Vue({
				el:'#root',
				data:{
					student:{
						name:'tom',
						age:18,
						hobby:['抽烟','喝酒','烫头'],
						friends:[
							{name:'jerry',age:35},
							{name:'tony',age:36}
						]
					}
				},
				methods: {
					addSex(){
						// Vue.set(this.student,'sex','男')
						this.$set(this.student,'sex','男')
					},
					addFriend(){
						this.student.friends.unshift({name:'jack',age:70})
					},
					updateFirstFriendName(){
						this.student.friends[0].name = '张三'
					},
					addHobby(){
						this.student.hobby.push('学习')
					},
					updateHobby(){
						// this.student.hobby.splice(0,1,'开车')
						// Vue.set(this.student.hobby,0,'开车')
						this.$set(this.student.hobby,0,'开车')
					},
					removeSmoke(){
						this.student.hobby = this.student.hobby.filter((h)=>{
							return h !== '抽烟'
						})
					}
				}
			})
		</script>
	        ...
	
	```
	
### 收集表单数据

1. 若 是输入框用户可以输入 ，则`v-model`收集的是`value`值，用户输入的就是`value`值，用户输入的就是`value`值。
2. 若 是选择圆框 用户不可输入 则`v-model`收集的还是`value`值 且需要给标签配置`value`值
3. 若是勾选框则
	1. 没有配置`input`的`value`值，那么收集的就是`checked`(勾选 or 未勾选，是布尔值)
	2. 配置input的value属性：
		1. `v-model`的初始值是非数组，那么收集的就是`checked`(勾选 or 未勾选，是布尔值)
		2. `v-model`的初始值是数组，那么收集的的就是`value`组成的数组

备注：`v-model`的三个修饰属性:

+ `lazy`：失去焦点再收集数据
+ `number`：输入字符串转为有效的数字
+ `trim`：输入首尾空格过滤

```html
       ...
      <body>
		<!-- 准备好一个容器-->
		<div id="root">
			<form @submit.prevent="demo">
				账号：<input type="text" v-model.trim="userInfo.account"> <br/><br/>
				密码：<input type="password" v-model="userInfo.password"> <br/><br/>
				年龄：<input type="number" v-model.number="userInfo.age"> <br/><br/>
				性别：
				男<input type="radio" name="sex" v-model="userInfo.sex" value="male">
				女<input type="radio" name="sex" v-model="userInfo.sex" value="female"> <br/><br/>
				爱好：
				学习<input type="checkbox" v-model="userInfo.hobby" value="study">
				打游戏<input type="checkbox" v-model="userInfo.hobby" value="game">
				吃饭<input type="checkbox" v-model="userInfo.hobby" value="eat">
				<br/><br/>
				所属校区
				<select v-model="userInfo.city">
					<option value="">请选择校区</option>
					<option value="beijing">北京</option>
					<option value="shanghai">上海</option>
					<option value="shenzhen">深圳</option>
					<option value="wuhan">武汉</option>
				</select>
				<br/><br/>
				其他信息：
				<textarea v-model.lazy="userInfo.other"></textarea> <br/><br/>
				<input type="checkbox" v-model="userInfo.agree">阅读并接受<a 
                                     href="http://www.atguigu.com">《用户协议》</a>
				<button>提交</button>
			</form>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false

		new Vue({
			el:'#root',
			data:{
				userInfo:{
					account:'',
					password:'',
					age:18,
					sex:'female',
					hobby:[],
					city:'beijing',
					other:'',
					agree:''
				}
			},
			methods: {
				demo(){
					console.log(JSON.stringify(this.userInfo))
				}
			}
		})
	</script>
          ...
```

### 过滤器

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>过滤器</title>
		<script type="text/javascript" src="../js/vue.js"></script>
		<script src="https://cdn.bootcdn.net/ajax/libs/dayjs/1.10.6/dayjs.min.js"></script>
	</head>
	<body>
		<div id="root">
			<h2>时间</h2>
            <h3>当前时间戳：{{time}}</h3>
            <h3>转换后时间：{{time | timeFormater()}}</h3>
			<h3>转换后时间：{{time | timeFormater('YYYY-MM-DD HH:mm:ss')}}</h3>
			<h3>截取年月日：{{time | timeFormater() | mySlice}}</h3>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false
		//全局过滤器
		Vue.filter('mySlice',function(value){
			return value.slice(0,11)
		})
		new Vue({
            el:'#root',
            data:{
                time:1626750147900,
            },
			//局部过滤器
            filters:{
                timeFormater(value, str="YYYY年MM月DD日 HH:mm:ss"){
                    return dayjs(value).format(str)
                }
            }
        })
	</script>
</html>

```

效果：

![img](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/ae26b7438a92ea522859afff5ad96cba.png)

###### 总结：

过滤器：

+ 定义：对要显示的数据进行==特定格式化后再显示==(适合于一些简单逻辑处理)。
+ 语法：
  1. 注册过滤器：==Vue.filter(name,callback)==或 ==new Vue{filters:{}}==
  2. 使用过滤器：=={{ xxx | 过滤器名}}==或==v-bind:属性 = "xxx | 过滤器名"==

==注意==

1. 过滤器可以接收额外参数，多个过滤器也可以串联
2. 并没有改变原本的数据，而是产生新的对应的数据

### 内置指令

#### `v-text`指令

```html
<html>
	<head>
		<meta charset="UTF-8" />
		<title>v-text指令</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<div>你好，{{name}}</div>
			<div v-text="name"></div>
			<div v-text="str"></div>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false 
		
		new Vue({
			el:'#root',
			data:{
				name:'JOJO',
				str:'<h3>你好啊！</h3>'
			}
		})
	</script>
</html>
```

效果：

![image-20220912214903906](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220912214903906.png)

总结：

+ 之前学过的指令：
  + `v-bind`:单向绑定解析表达式，可简写为==`:`==
  + `v-model`:双向数据绑定
  + `v-for`:遍历数组、对象、字符串
  + `v-on`：绑定事件监听，可简写为 ==`@`==
  + `v-if`:条件渲染（动态控制节点是否存在）
  + `v-else`:条件渲染（动态控制节点是否存在）
  + `v-show`:条件渲染（动态控制节点是否存在）
+ `v-text`指令：
  + 作用：向其所在的节点中渲染文本内容
  + 与插值语法的区别：`v-text` 会替换掉节点中的内容，插值语法不会

#### `v-html`指令

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>v-html指令</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<div>Hello，{{name}}</div>
			<div v-html="str"></div>
			<div v-html="str2"></div>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		new Vue({
			el:'#root',
			data:{
				name:'JOJO',
				str:'<h3>你好啊！</h3>',
				str2:'<a href=javascript:location.href="http://www.baidu.com?"+document.cookie>兄弟我找到你想要的资源了，快来！</a>',
			}
		})
	</script>
</html>

```

效果：

![image-20220912215831948](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220912215831948.png)

总结：

`v-html`指令：

1. 作用：向指定节点中渲染包含html结构的内容
2. 与插值语法的区别：
   1. `v-html`会替换节点中所有的的内容，而插值语法不会
   2. `v-html` 可以识别`html`结构
3. ==严重问题==：`v-html`有==安全问题==！！！
   1. 再网站上动态渲染任意html是非常危险的，容易打导致`xss`攻击
   2. 一定要在可信任的额内容上使用`v-html`，永远不要在用户提交的内容上使用！！

#### v-cloak指令

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>v-cloak指令</title>
		<style>
			[v-cloak]{
				display:none;
			}
		</style>
	</head>
	<body>
		<div id="root">
			<h2 v-cloak>{{name}}</h2>
		</div>
		<script type="text/javascript" src="../js/vue.js"></script>
	</body>
	
	<script type="text/javascript">
		Vue.config.productionTip = false
		
		new Vue({
			el:'#root',
			data:{
				name:'尚硅谷'
			}
		})
	</script>
</html>

```

效果：

![image-20220912220412924](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220912220412924.png)

总结：

`v-cloak`指令(没有值)

1. 本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删除掉`v-cloak`属性
2. 使用css配合`v-cloak` 可以解决网速慢是页面出现{{xxx}}的问题

#### `v-once`指令

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>v-once指令</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<h2 v-once>n初始化的值是：{{n}}</h2>
            <h2>n现在的值是：{{n}}</h2>
            <button @click="n++">点我n+1</button>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false 
		
		new Vue({
			el:'#root',
			data:{
				n:1
			}
		})
	</script>
</html>

```

效果：

![image-20220912220728147](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220912220728147.png)

总结：

`v-once`指令：

1. `v-once`所在节点在初次动态渲染后，就视为静态内容了
2. 以后数据发生改变不会引起`v-once`所在结构的更新，可以用于优化性能

#### `v-pre`指令

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>v-pre指令</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<h2 v-pre>Vue其实很简单</h2>
			<h2>当前的n值是:{{n}}</h2>
			<button @click="n++">点我n+1</button>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false

		new Vue({
			el:'#root',
			data:{
				n:1
			}
		})
	</script>
</html>

```

效果：


![image-20220912221049833](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220912221049833.png)

总结：

`v-pre`指令：

1. 跳过其所在节点编译过程
2. 可利用它跳过：没有使用指令语法，没有插值语法的节点，加快编译

###自定义指令

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>自定义指令</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
    <!-- 
		需求1：定义一个v-big指令，和v-text功能类似，但会把绑定的数值放大10倍。
		需求2：定义一个v-fbind指令，和v-bind功能类似，但可以让其所绑定的input元素默认获取焦点。
	-->
	<body>
		<div id="root">
			<h2>当前的n值是：<span v-text="n"></span> </h2>
			<h2>放大10倍后的n值是：<span v-big="n"></span> </h2>
			<button @click="n++">点我n+1</button>
			<hr/>
			<input type="text" v-fbind:value="n">
		</div>
	</body>
	
	<script type="text/javascript">
		Vue.config.productionTip = false

		new Vue({
			el:'#root',
			data:{
				n:1
			},
			directives:{
                //big函数何时会被调用？1.指令与元素成功绑定时（一上来） 2.指令所在的模板被重新解析时
				big(element,binding){
					console.log('big',this) //注意此处的this是window
					element.innerText = binding.value * 10
				},
				fbind:{
					//指令与元素成功绑定时（一上来）
					bind(element,binding){
						element.value = binding.value
					},
					//指令所在元素被插入页面时
					inserted(element,binding){
						element.focus()
					},
					//指令所在的模板被重新解析时
					update(element,binding){
						element.value = binding.value
					}
				}
			}
		})
	</script>
</html>

```

效果：

![image-20220912221228497](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220912221228497.png)

总结：

1. 自定义指令语法：

   1. 局部变量

      1.  ```javascript
         new Vue({
              directives:{指令名:配置对象}   
         })
         ```

      2.  ```javascript
         new Vue({
          directives:{指令名:回调函数}   
          })

​	2. 全局指令：

​		1.vue.directive(指令名，配置对象)

​		2.vue.directice(指令命，回调函数)

2. 配置对象中常用的3个回调函数：
   1. bind(element,binding):指令于元素成功绑定时调用 （即一上来就调用）
   2. inserted(element,binding):指令所在元素被插入页面时调用
   3. update(element,binding):指令所在模板结构被重新解析是调用
3. 备注：
   1. 指令定义时不加`v-`，但使用时要加`v-`
   2. 指令名如果是多个单词，要使用kebab-kase(即使用-连接 `big-number`)命名方式，不要使用camelCase(即小驼峰 ~~`bigName`~~)命名

###`Vue`生命周期

#### 引出生命周期

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>引出生命周期</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<h2 v-if="a">你好啊</h2>
			<h2 :style="{opacity}">欢迎学习Vue</h2>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false 

		 new Vue({
			el:'#root',
			data:{
				a:false,
				opacity:1
			},
			mounted(){
				console.log('mounted',this)
				setInterval(() => {
					this.opacity -= 0.01
					if(this.opacity <= 0) this.opacity = 1
				},16)
			},
		})
	</script>
</html>

```

效果：

![image-20220913090548945](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220913090548945.png)

总结：

生命周期：

1. 又名：生命周期回调函数，生命周期函数，生命周期钩子
2. 是什么：Vue在关键时刻帮我们调用的一些特殊名称的函数
3. 生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的
4. 生命周期函数中的`this`指向的是`Vm` 或 组件实例对象

#### 分析生命周期

![image-20220913091002851](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220913091002851.png)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>分析生命周期</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<h2 v-text="n"></h2>
			<h2>当前的n值是：{{n}}</h2>
			<button @click="add">点我n+1</button>
			<button @click="bye">点我销毁vm</button>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false

		new Vue({
			el:'#root',
			// template:`
			// 	<div>
			// 		<h2>当前的n值是：{{n}}</h2>
			// 		<button @click="add">点我n+1</button>
			// 	</div>
			// `,
			data:{
				n:1
			},
			methods: {
				add(){
					console.log('add')
					this.n++
				},
				bye(){
					console.log('bye')
					this.$destroy()
				}
			},
			watch:{
				n(){
					console.log('n变了')
				}
			},
			beforeCreate() {
				console.log('beforeCreate')
			},
			created() {
				console.log('created')
			},
			beforeMount() {
				console.log('beforeMount')
			},
			mounted() {
				console.log('mounted')
			},
			beforeUpdate() {
				console.log('beforeUpdate')
			},
			updated() {
				console.log('updated')
			},
			beforeDestroy() {
				console.log('beforeDestroy')
			},
			destroyed() {
				console.log('destroyed')
			},
		})
	</script>
</html>

```

#### 总结生命周期

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>引出生命周期</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<h2 :style="{opacity}">欢迎学习Vue</h2>
			<button @click="opacity = 1">透明度设置为1</button>
			<button @click="stop">点我停止变换</button>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false 

		 new Vue({
			el:'#root',
			data:{
				opacity:1
			},
			methods: {
				stop(){
					this.$destroy()
				}
			},
			mounted(){
				console.log('mounted',this)
				this.timer = setInterval(() => {
					console.log('setInterval')
					this.opacity -= 0.01
					if(this.opacity <= 0) this.opacity = 1
				},16)
			},
			beforeDestroy() {
				clearInterval(this.timer)
				console.log('vm即将驾鹤西游了')
			},
		})
	</script>
</html>

```

![image-20220913091117319](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220913091117319.png)

总结：

常用的生命周期钩子：

1. `mounted`：发送ajax请求、启动定时器、绑定自定义事件、订阅消息等初始化操作
2. `beforeDestroy`：清楚定时器、解绑自定义事件、取消订阅消息等收尾工作

关于销毁Vue实例：

1. 销毁后借助Vue开发者工具看不到任何信息
2. 销毁自定义事件会失效，但原生DOM事件依然有效
3. 一般不会再`beforeDestory`操作数据，因为几遍操作数据，也不会触发更新流程了

## Vue组件化编程

### 模块与组件、模块化与组件化

#### 模块

1. 理解：向外提供特定功能的js程序，一般就是一个js文件
2. 为什么：js文件很多很复杂
3. 作用：复用js，简化js的编写，提高js运行效率

#### 组件

1. 定义：用来实现局部功能的代码和资源的集合
2. 为什么：一个界面的功能很复杂
3. 作用：复用编码，简化项目编码，提高运行效率

#### 模块化

当应用中的js都以模块来编写，那这个应用就是一个模块化的应用

#### 组件化

当应用中的功能都是多组件的方式来编写的，那这个应用就是一个组件化的应用

### 非D单文件组件

#### 基本使用

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>基本使用</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<h1>{{msg}}</h1>
			<hr>
			<!-- 第三步：编写组件标签 -->
			<school></school>
			<hr>
			<!-- 第三步：编写组件标签 -->
			<student></student>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false

		//第一步：创建school组件
		const school = Vue.extend({
            //组件定义时，一定不要写el配置项，因为最终所有的组件都要被一个vm管理，由vm决定服务于哪个容器。
			template:`
				<div class="demo">
					<h2>学校名称：{{schoolName}}</h2>
					<h2>学校地址：{{address}}</h2>	
				</div>
			`,
			data(){
				return {
					schoolName:'尚硅谷',
					address:'北京昌平'
				}
			}
		})

		//第一步：创建student组件
		const student = Vue.extend({
			template:`
				<div>
					<h2>学生姓名：{{studentName}}</h2>
					<h2>学生年龄：{{age}}</h2>
				</div>
			`,
			data(){
				return {
					studentName:'JOJO',
					age:20
				}
			}
		})
		
		//创建vm
		new Vue({
			el:'#root',
			data:{
				msg:'你好，JOJO！'
			},
			//第二步：注册组件（局部注册）
			components:{
				school,
				student
			}
		})
	</script>
</html>

```

效果：

![image-20220913093447192](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220913093447192.png)

总结：

+ Vue中使用组件的三大步骤：

  + 定义组件
  + 注册组件
  + 使用组件

+ 如何定义一个组价？

  使用`Vue.extend(options)`创建，其中`options`和`new Vue(options)`时传入的`options`几乎一样,但也有区别：

  + el不要写，为什么？
    + 最终所有的组件都要经过一个`VM`管理，由`vm` 决定服务于那个容器
  + data必须写成函数，为什么？
    + 避免组件别复用，数据存在引用关系

+ 如何注册组件？

  + 局部注册：`new Vue`的时候传入`components`选项
  + 全局注册：`Vue.component`(组件名，组件)

+ 编写组件标签：`<school></school>`

#### 组件注意事项

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>组件注意事项</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<h1>{{msg}}</h1>
			<school></school>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false
		
		const school = Vue.extend({
			name:'atguigu',
			template:`
				<div>
					<h2>学校名称：{{name}}</h2>	
					<h2>学校地址：{{address}}</h2>	
				</div>
			`,
			data(){
				return {
					name:'尚硅谷',
					address:'北京'
				}
			}
		})

		new Vue({
			el:'#root',
			data:{
				msg:'欢迎学习Vue!'
			},
			components:{
				school
			}
		})
	</script>
</html>

```

效果：

![image-20220913094453690](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220913094453690.png)

总结：

+ 关于组件名

  + 一个单词组成：

    + `school`
    + `School`
+ 多个单词组成：
  
  + `kebab-case`:`my-school`
    + `CamelCase`:`MySchool`
  + 备注：

    + 组件名尽可能回避HTML中已有的元素名称，例如：h2、H2都不行
  + 可以使用name配置项指定组件在开发者工具中呈现的名字
+ 关于组件标签：
  1. 第一种：`<school></school>`
  2. 第二种：`<school/>`
  3. 备注：不使用脚手架时，<school/>会导致后续组件不能渲染
+ 一种简写方式：`const school = Vue.extend(options)` 可简写为：`const school = options`

####  组件的嵌套

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>组件的嵌套</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false
		
		//定义student组件
		const student = Vue.extend({
			template:`
				<div>
					<h2>学生名称：{{name}}</h2>	
					<h2>学生年龄：{{age}}</h2>	
				</div>
			`,
			data(){
				return {
					name:'JOJO',
					age:20
				}
			}
		})

		//定义school组件
		const school = Vue.extend({
			template:`
				<div>
					<h2>学校名称：{{name}}</h2>	
					<h2>学校地址：{{address}}</h2>	
					<student></student>
				</div>
			`,
			components:{
				student
			},
			data(){
				return {
					name:'尚硅谷',
					address:'北京'
				}
			}
		})

		//定义hello组件
		const hello = Vue.extend({
			template:`
				<h1>{{msg}}</h1>
			`,
			data(){
				return {
					msg:"欢迎学习尚硅谷Vue教程！"
				}
			}
		})

		//定义app组件
		const app = Vue.extend({
			template:`
				<div>
					<hello></hello>
					<school></school>
				</div>
			`,
			components:{
				school,
				hello
			}
		})

		//创建vm
		new Vue({
			template:`
				<app></app>
			`,
			el:'#root',
			components:{
				app
			}
		})
	</script>
</html>

```

效果：

![image-20220913095400448](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220913095400448.png)

#### VueComponent

关于VueComponent：

1. school组件本质是一个名为`VueComponent`的构造函数，且不是程序员定义的，是`Vue.extend`生成的

2. 我们只需要写`<school/>`或`<school></school>`，Vue解析时会帮我们创建school组件的实例对象，即Vue帮我们执行的：`new VueComponent(options)`

3. 特别注意：每次调用`Vue.extend`，返回的都是一个全新的VueComponent！

4. 关于this指向：

1. 1. 组件配置中：`data`函数、`methods`中的函数、`watch`中的函数、`computed`中的函数 它们的this均是VueComponent实例对象
   2. `new Vue(options)`配置中：`data`函数、`methods`中的函数、`watch`中的函数、`computed`中的函数 它们的this均是Vue实例对象

5. `VueComponent`的实例对象，以后简称vc（也可称之为：组件实例对象）

`Vue`的实例对象，以后简称vm

只有在本笔记中`VueComponent`的实例对象才简称为vc

#### 一个重要的内置关系

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>一个重要的内置关系</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<school></school>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false
		Vue.prototype.x = 99

		const school = Vue.extend({
			name:'school',
			template:`
				<div>
					<h2>学校名称：{{name}}</h2>	
					<h2>学校地址：{{address}}</h2>	
					<button @click="showX">点我输出x</button>
				</div>
			`,
			data(){
				return {
					name:'尚硅谷',
					address:'北京'
				}
			},
			methods: {
				showX(){
					console.log(this.x)
				}
			},
		})

		const vm = new Vue({
			el:'#root',
			data:{
				msg:'你好'
			},
			components:{school}
		})
	</script>
</html>

```

效果：

![image-20220913095655367](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220913095655367.png)

总结：

![image-20220913095706414](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220913095706414.png)

1. 一个重要的内置关系：`VueComponent.prototype.__proto__ === Vue.prototype`
2. 为什么要有这个关系：让组件实例对象（vc）可以访问到 Vue 原型上的属性、方法

### 单文件组件

`School.vue`:

```vue

<template>
•	    <div id='Demo'>
•	        <h2>学校名称：{{name}}</h2>
•	        <h2>学校地址：{{address}}</h2>
•	        <button @click="showName">点我提示学校名</button>
•	    </div>
•	</template>
•	
•	<script>
•	    export default {
•	        name:'School',
•	        data() {
•	            return {
•	                name:'尚硅谷',
•	                address:'北京'
•	            }
•	        },
•	        methods: {
•	            showName(){
•	                alert(this.name)
•	            }
•	        },
•	    }
•	</script>
•	
•	<style>
•	    #Demo{
•	        background: orange;
•	    }
•	</style>
```

`Student.vue`:

```vue
•	<template>
•	    <div>
•	        <h2>学生姓名：{{name}}</h2>
•	        <h2>学生年龄：{{age}}</h2>
•	    </div>
•	</template>
•	
•	<script>
•	    export default {
•	        name:'Student',
•	        data() {
•	            return {
•	                name:'JOJO',
•	                age:20
•	            }
•	        },
•	    }
•	</script>
```

`App.vue`:

```vue
•	<template>
•	    <div>
•	        <School></School>
•	        <Student></Student>
•	    </div>
•	</template>
•	
•	<script>
•	    import School from './School.vue'
•	    import Student from './Student.vue'
•	
•	    export default {
•	        name:'App',
•	        components:{
•	            School,
•	            Student
•	        }
•	    }
•	</script>
```

`main.js`:

```vue
•	import App from './App.vue'
•	
•	new Vue({
•	    template:`<App></App>`,
•	    el:'#root',
•	    components:{App}
•	})
•	index.html
•	<!DOCTYPE html>
•	<html lang="en">
•	<head>
•	    <meta charset="UTF-8">
•	    <meta http-equiv="X-UA-Compatible" content="IE=edge">
•	    <meta name="viewport" content="width=device-width, initial-scale=1.0">
•	    <title>单文件组件练习</title>
•	</head>
•	<body>
•	    <div id="root"></div>
•	    <script src="../../js/vue.js"></script>
•	    <script src="./main.js"></script>
•	</body>
•	</html>
```

## 使用`Vue CLI`脚手架

### 初始化`Vue CLI` 脚手架

####说明

1. Vue脚手架是`Vue`官方提供的标准化开发工具（开发平台）
2. 最新的版本是4.x
3. 文档：`Vue CLI`

#### 具体步骤

1. 配置淘宝镜像：`npm config set registry http://registry.npm.taobao.org`
2. 全局安装`@vue/cli`: `npm install -g @vue/cli`
2. 切换到你要创建的目录，然后使用命令创建项目：`vue create xxx`
2. 选择vue版本
2. 启动项目：`npm run serve`
2. 暂停项目：`ctr+c`

`Vue` 脚手架隐藏了所有`webpack`相关的配置，若想查看`webpakc`配置，请执行：`vue inspect > output.js`

#### 分析脚手架结构

脚手架文件结构：

```tex
.文件目录
├── node_modules 
├── public
│   ├── favicon.ico: 页签图标
│   └── index.html: 主页面
├── src
│   ├── assets: 存放静态资源
│   │   └── logo.png
│   │── component: 存放组件
│   │   └── HelloWorld.vue
│   │── App.vue: 汇总所有组件
│   └── main.js: 入口文件
├── .gitignore: git版本管制忽略的配置
├── babel.config.js: babel的配置文件
├── package.json: 应用包配置文件 
├── README.md: 应用描述文件
└── package-lock.json: 包版本控制文件
```

`src/components/School.vue`:

```vue
<template>
    <div id='Demo'>
        <h2>学校名称：{{name}}</h2>
        <h2>学校地址：{{address}}</h2>
        <button @click="showName">点我提示学校名</button>
    </div>
</template>

<script>
    export default {
        name:'School',
        data() {
            return {
                name:'尚硅谷',
                address:'北京'
            }
        },
        methods: {
            showName() {
                alert(this.name)
            }
        },
    }
</script>

<style>
    #Demo{
        background: orange;
    }
</style>

```

`src/components/Student.vue`:

```vue
<template>
    <div>
        <h2>学生姓名：{{name}}</h2>
        <h2>学生年龄：{{age}}</h2>
    </div>
</template>

<script>
    export default {
        name:'Student',
        data() {
            return {
                name:'JOJO',
                age:20
            }
        },
    }
</script>

```

`src/App.vue`:

```vue
<template>
    <div>
        <School></School>
        <Student></Student>
    </div>
</template>

<script>
    import School from './components/School.vue'
    import Student from './components/Student.vue'

    export default {
        name:'App',
        components:{
            School,
            Student
        }
    }
</script>
```

`src/main.js`:

```javascript
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
    el:'#app',
    render: h => h(App),
})
```

`public/index.html`:

```html
<!DOCTYPE html>
<html lang="">
    <head>
        <meta charset="UTF-8">
        <!-- 针对IE浏览器的特殊配置，含义是让IE浏览器以最高渲染级别渲染页面 -->
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <!-- 开启移动端的理想端口 -->
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <!-- 配置页签图标 -->
        <link rel="icon" href="<%= BASE_URL %>favicon.ico">
        <!-- 配置网页标题 -->
        <title><%= htmlWebpackPlugin.options.title %></title>
    </head>
    <body>
        <!-- 容器 -->
        <div id="app"></div>
    </body>
</html>
```

效果：



![image-20220914172209136](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220914172209136.png)

#### render函数

`src/App.vue`:

```vue
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
    el:'#app',
    // 简写形式
	render: h => h(App),
    // 完整形式
	// render(createElement){
	//     return createElement(App)
	// }
})
```

总结：

关于不同版本的函数：

1. `vue.js`与`vue.runtime.xxx.js`的区别：
   1. `vue.js` 是完整的`Vue`，包含：核心功能+模板解析器
   2. `vue.runtime.xxx.js`是运行版本的`Vue`，只包含核心功能，没有模板解析器
2. 因为`vue.runtime.xxx.js`没有模板解析器，所以不能使用`template`配置项，需要使用`render`函数接收到的`createElement`函数去指定具体内容

#### 修改默认配置

+ `vue.config.js` 是一个可选配置文件，如果项目的根目录(`package`同级)中存在这个文件，那么他会被`@vue/cli-service`自动加载
+ 使用`vue.config.js`可以对脚手架进行个性定制，详细配置请参考[Vue CLI ](https://cli.vuejs.org/zh/config/#vue-config-js)

### ref属性

```vue
<template>
    <div>
        <h1 ref="title">{{msg}}</h1>
        <School ref="sch"/>
        <button @click="show" ref="btn">点我输出ref</button>
    </div>
</template>

<script>
    import School from './components/School.vue'
    export default {
        name:'App',
        components: { School },
        data() {
            return {
                msg:'欢迎学习Vue！'
            }
        },
        methods:{
            show(){
                console.log(this.$refs.title)
                console.log(this.$refs.sch)
                console.log(this.$refs.btn)
            }
        }
    }
</script>

```

效果：

![image-20220914173614608](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220914173614608.png)

总结：
`ref`属性：

1. 被用来给元素或子组件注册引用信息（id的替代者）
2. 应用在`html`标签上获取的是真实DOM元素。应用在组件标签上获取的就是组件实例对象(`VC`)
3. 使用方法：
   1. 打标识:`<h1 ref="xxx"></h1>`或`<School ref="xxx"></School >`
   2. 获取：`this.$refs.xxx`

### props配置项

`src/App.vue`:

```vue
<template>
    <div>
        <Student name="JOJO" sex="男酮" :age="20" />
    </div>
</template>

<script>
    import Student from './components/Student.vue'
    export default {
        name:'App',
        components: { Student },
    }
</script>
```

`src/components/Student.vue`:

```vue
<template>
    <div>
        <h1>{{msg}}</h1>
        <h2>学生姓名：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2>
        <h2>学生年龄：{{age}}</h2>     
    </div>
</template>

<script>
    export default {
        name:'Student',
        data() {
            return {
                msg:"我是一大男的~",
            }
        },
        // 简单声明接收
		// props:['name','age','sex']

        // 接收的同时对数据进行类型限制
		/* props:{
			name:String,
			age:Number,
			sex:String
		} */

        // 接收的同时对数据进行类型限制 + 指定默认值 + 限制必要性
		props:{
			name:{
				type:String,
				required:true,
			},
			age:{
				type:Number,
				default:99
			},
			sex:{
				type:String,
				required:true
			}
		}
    }
</script>

```

总结：

`props`配置项：

1. 功能：让组件接受外部传过来的数据

2. 传递数据：`<Studnet name='xxx'/>`

3. 接受数据：

   1. 第一种方式(只接受)：`props:['name']`

   2. 第二种方式(限定数据类型)：`props:{name:String}`

   3. 第三种方式(限定数据类型、限定必要性、指定默认值)：

      1. >`props:{`
         >
         >​	`	name:{`
         >
         >​		`type:String` //类型
         >
         >​		`required:true`//必要性
         >
         >​		`default:'join'`//默认值
         >
         >​	`}`
         >
         >`}`
         >
         >`props`是只读的，`Vue`底层会监测你对`props`的修改，如果进行修改，就会触发警报，若业务需求确实需要修改，那么请复制`props`的内容到`data`中，然后去修改`data`中的数据

​		

### `mixin`混入

局部混入：

`src/mixin.js`:

```javascript
export const mixin = {
    methods: {
        showName() {
            alert(this.name)
        }
    },
    mounted() {
        console.log("你好呀~")
    }
}
```

`src/components/School.vue`:

```vue
<template>
    <div>
        <h2 @click="showName">学校姓名：{{name}}</h2>
        <h2>学校地址：{{address}}</h2>   
    </div>
</template>

<script>
    //引入混入
    import {mixin} from '../mixin'
    
    export default {
        name:'School',
        data() {
            return {
                name:'尚硅谷',
				address:'北京'
            }
        },
        mixins:[mixin]
    }
</script>
```

`src/components/Student.vue`:



```vue
<template>
    <div>
        <h2 @click="showName">学生姓名：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2>   
    </div>
</template>
 
<script>
    //引入混入
    import {mixin} from '../mixin'
    
    export default {
        name:'Student',
        data() {
            return {
                name:'JOJO',
                               sex:'男'
            }
        },
               mixins:[mixin]
    }
</script>
```

`src/App.vue`:

```vue
<template>
    <div>
        <School/>
        <hr/>
        <Student/>
    </div>
</template>
 
<script>
    import Student from './components/Student.vue'
    import School from './components/School.vue'
 
    export default {
        name:'App',
        components: { Student,School },
    }
</script>
```

效果：

![image-20220915090239263](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220915090239263.png)

全局混入：

`src/main.js`:

```js
import Vue from 'vue'
import App from './App.vue'
import {mixin} from './mixin'
 
Vue.config.productionTip = false
Vue.mixin(mixin)
 
new Vue({
    el:"#app",
    render: h => h(App)
})
```

效果：

![image-20220915090410185](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220915090410185.png)

------

总结：

`mixin`(混入)：

1. 功能：可以把多个组件共用的配置提取成一个混入对象

2. 使用方式：

   1. 定义混入：

      1. > const mixin ={
         >
         > ​	data(){....},
         >
         > ​	methods:{....}
         >
         > ​	.......
         >
         > }
      
   2. 使用混入：

        1. 全局混入：`Vue.mixin(xxx)`
        2. 局部混入：`mixins:['xxx']`

3. 备注：

   1. 组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”，在发生冲突时以组件优先。

      ```javascript
      1.  var mixin = {
      2.     data: function () {
      3.             return {
      4.                     message: 'hello',
      5.              foo: 'abc'
      6.             }
      7.     }
      8.  }
      9.   
      10.new Vue({
      11.   mixins: [mixin],
      12.   data () {
      13.           return {
      14.                   message: 'goodbye',
      15.            bar: 'def'
      16.           }
      17.    },
      18.   created () {
      19.           console.log(this.$data)
      20.           // => { message: "goodbye", foo: "abc", bar: "def" }
      21.   }
      22.})
      ```
      
   2. 同名生命周期钩子将合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子之前调用
   
      ```javascript
      1.  var mixin = {
      2.     created () {
      3.             console.log('混入对象的钩子被调用')
      4.     }
      5.  }
      6.   
      7.  new Vue({
      8.     mixins: [mixin],
      9.     created () {
      10.           console.log('组件钩子被调用')
      11.   }
      12.})
      13. 
      14.// => "混入对象的钩子被调用"
      15.// => "组件钩子被调用"
      ```

### `plugin`插件

`src/plugin.js`:

```javascript
export default {
	install(Vue,x,y,z){
		console.log(x,y,z)
		//全局过滤器
		Vue.filter('mySlice',function(value){
			return value.slice(0,4)
		})

		//定义混入
		Vue.mixin({
			data() {
				return {
					x:100,
					y:200
				}
			},
		})

		//给Vue原型上添加一个方法（vm和vc就都能用了）
		Vue.prototype.hello = ()=>{alert('你好啊')}
	}
}
```

`src/main.js`:

```vue
import Vue from 'vue'
import App from './App.vue'
import plugin from './plugin'

Vue.config.productionTip = false
Vue.use(plugin,1,2,3)

new Vue({
    el:"#app",
    render: h => h(App)
})
src/components/School.vue:
<template>
    <div>
        <h2>学校姓名：{{name | mySlice}}</h2>
        <h2>学校地址：{{address}}</h2>   
    </div>
</template>

<script>
    export default {
        name:'School',
        data() {
            return {
                name:'尚硅谷atguigu',
				address:'北京'
            }
        }
    }
</script>
```

`src/components/Student.vue`:

```vue
<template>
    <div>
        <h2>学生姓名：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2> 
        <button @click="test">点我测试hello方法</button>  
    </div>
</template>

<script>
    export default {
        name:'Student',
        data() {
            return {
                name:'JOJO',
				sex:'男'
            }
        },
        methods:{
            test() {
                this.hello()
            }
        }
    }
</script>
```

效果：

![image-20220915092651518](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220915092651518.png)

总结：

插件：

1. 功能：用于增强`Vue`

2. 本质：包含`install` 方法的一个对象，`install`的第一个参数是 `Vue` ，第二个以后的参数是插件使用者传递的数据

3. 定义插件：

   ```javascript
   4.	plugin.install = function (Vue, options) {
   5.	        // 1. 添加全局过滤器
   6.	        Vue.filter(....)
   7.	    
   8.	        // 2. 添加全局指令
   9.	        Vue.directive(....)
   10.	    
   11.	        // 3. 配置全局混入
   12.	        Vue.mixin(....)
   13.	    
   14.	        // 4. 添加实例方法
   15.	        Vue.prototype.$myMethod = function () {...}
   16.	        Vue.prototype.$myProperty = xxxx
   17.	    }
   ```

4. 使用插件：`Vue.use`(`plugin`)

### scoped样式

`src/components/School.vue`:



```vue
<template>
    <div class="demo">
        <h2>学校姓名：{{name}}</h2>
        <h2>学校地址：{{address}}</h2>   
    </div>
</template>
 
<script>
    export default {
        name:'School',
        data() {
            return {
                name:'尚硅谷',
                               address:'北京'
            }
        }
    }
</script>
 
<style scoped>
    .demo{
        background-color: blueviolet;
    }
</style>
```

`src/components/Student.vue`:

```vue
<template>
    <div class="demo">
        <h2>学生姓名：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2> 
    </div>
</template>

<script>
    export default {
        name:'Student',
        data() {
            return {
                name:'JOJO',
				sex:'男'
            }
        }
    }
</script>

<style scoped>
    .demo{
        background-color: chartreuse;
    }
</style>
```



`src/App.vue`:

```vue
<template>
    <div>
        <School/>
        <Student/>
    </div>
</template>
 
<script>
    import Student from './components/Student.vue'
    import School from './components/School.vue'
 
    export default {
        name:'App',
        components: { Student,School },
    }
</script>
```

效果：

![image-20220915093649000](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/image-20220915093649000.png)

总结：

scoped：

+ 作用：让样式在局部生效，防止冲突
+ 写法：<style scoped>

### Todo-List 案例

组件化编码流程

1. 拆分静态组件：组件要按照功能点拆分，命名不要与`html`元素冲突
2. 实现动态组件：考虑好数的存放位置，数据是一个组件再用，还是一些组件再用
   + 一个组件再用：放在组件自身即可
   + 一些组件再用：放在他们共同的父组件上
3. 实现交互：从绑定事件开始

`props`适用于

+ 父组件 ==> 子组件 通信
+ 子组件 ==> 父组件 通信 （要求父组件先给子组件一个函数）

使用`V-model`时要切记：`v-model`绑定的值不能是`props`传过来的值，应为`props`不能被修改

`props`传过来的若是对象类型的值，修改对象中的属性时`Vue`不会报错，但不推荐这样做

`srv/App.Vue`

```vue
<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader :receive="receive" ></MyHeader>
        <List
            :todos="todos"
            :updateTodo="updateTodo"
            :deleteTodo="deleteTodo">
        </List>
        <Footer :todos="todos" :deleteAll="deleteAll"></Footer>
      </div>
    </div>
  </div>
</template>

<script>
import MyHeader from './components/MyHeader';
import List from './components/List';
import Footer from './components/Footer';
export default {
  name: 'App',
  components:{MyHeader,List,Footer},
  data(){
    return{
      todos:[
        {id:'001',title:'唱',done:true},
        {id:'002',title:'跳',done:false},
        {id:'003',title:'Rap',done:true},
      ]
    }
  },
  methods:{
    //添加 任务
    receive(todo){
      this.todos.unshift(todo)
    },
    //修改任务状态
    updateTodo(id){
      this.todos.forEach((todo)=>{
        if(todo.id === id){
          todo.done = !todo.done
        }
      })
    },
    //删除 单个
    deleteTodo(id){
      this.todos.forEach((todo)=>{
        if(todo.id === id){
            // this.todos.delete()
          // this.todos.
          this.todos = this.todos.filter(t =>{
            return t.id !== id
          })
          console.log("我要删除了")
        }
      })
    },
    //删除完成任务
    deleteAll(){
      if (confirm("确定吗？")) {
        this.todos = this.todos.filter(todo => {
          return !todo.done
        })
      }
    }
  }
}
</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

/*header*/




/*main*/

/*item*/


/*footer*/


</style>

```

`src/components/MyHeader.vue`

```vue
<template>
  <div class="todo-header">
    <input type="text" placeholder="请输入你的任务名称，按回车键确认" @keyup.enter="add"/>
  </div>
</template>

<script>
import {nanoid} from 'nanoid'
export default {
  name: "MyHeader",
  props:['receive'],
  methods:{
    //添加一个Todo
    add(e){
      // 输入值不为空时
      if (e.target.value!=='') {
        //使用nanoid最为key 用户输入的值为 title  done:false 即为 默认不勾选
        const todoObj = {
          id: nanoid(),
          title: e.target.value,
          done: false
        }
        // 将创建好的Todo对象传递给父组件 App.Vue
        this.receive(todoObj)
        //重置输入框
        e.target.value = ''
      }
      // 值为空时
      else  alert("请输入值")
    }
  }
}
</script>

<style scoped>
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}
.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
}
</style>

```

`src/components/List.vue`

```vue
<template>
  <ul class="todo-main">
<!--   传递参数给子组件Item-->
    <Item
        v-for="todo in todos"
        :key="todo.id" :todo="todo"
        :updateTodo="updateTodo"
        :deleteTodo="deleteTodo">
    </Item>
  </ul>
</template>

<script>
import Item from "./Item";
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "List",
  components:{Item},
  props:['todos','updateTodo','deleteTodo']
}
</script>

<style scoped>
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
</style>
```

`src/components/Item.vue`

```vue
<template>
<div>
  <li>
    <label>
      <input type="checkbox" :checked="todo.done" @click="handleCheck(todo.id)"/>
      <span>{{ todo.title }}</span>
    </label>
    <button class="btn btn-danger"  @click="deleteTodos(todo.id)">删除</button>
  </li>
</div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Item",
  props:['todo','updateTodo','deleteTodo'],
  methods:{
    //点击时调用App.vue的修改方法 根据ID修改勾选状态
    handleCheck(id){
      this.updateTodo(id)
    },
    //点击时调用App.vue的删除方法 根据ID删除Todo
    deleteTodos(id){
      if (confirm("确定吗？"))
      this.deleteTodo(id)
    }
  }
}
</script>

<style scoped>
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}
li:hover button{
  display:block ;
}
</style>

```

`src/components/Footer.vue`

```vue
<template>
  <div class="todo-footer" v-show="isAll">
    <label>                    
                            <!--匹配勾选任务 与 总任务 -->
      <input type="checkbox" :checked="isAll===doneTotal " @click="checkedAll"/>
    </label>
    <span>
          <span>已完成{{doneTotal}}</span> / 全部{{todos.length}}
        </span>
    <button class="btn btn-danger" @click="deleteCompleted">清除已完成任务</button>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Footer",
  props:['todos','deleteAll'],
  methods:{
    deleteCompleted(){
      this.deleteAll()
    },
    //  全勾选/全不勾选
    checkedAll(e){
      if (e.target.checked) {
        this.todos.forEach(todo => {
          todo.done = true
        })
      }
      else {
        this.todos.forEach(todo => {
          todo.done = false
        })
      }
    }
  },
  computed:{
    //计数 勾选Todo数
    doneTotal(){
      return this.todos.reduce((pre, c) => {
        return pre + (c.done ? 1 : 0)
      }, 0)
    },
    //返回 当前任务数量
    isAll(){
      // 当任务数为零时返回false 实现没任务时 勾选框为未勾选
      if (this.todos.length!== 0) {
        return this.todos.length
      }
      else return false
    }
  },
}
</script>

<style scoped>
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>

```

效果：

![Todo](https://github.com/jmoreya/VueNote/blob/master/vue.js.assets/Todo-16640338644411.gif)

###   WebStorage本地存储 

#### WebStorage(js本地存储)

存储内容大小一般为`5MB`左右(不同游览器可能不同)

游览器段通过`Windows.sessionStorage`和`Windows.localStorage`属性来实现本地存储机制

相关`API`

+ `xxxStorage.setItem('key','value')`该方法接受一个键和值作为一个参数，会把键值对添加到存储中，如果键名存在，则更习惯其对应的值
+ `xxxStorage.getItem('key')`该方法接受一个键名作为一个参数，返回键名对应的值
+ `xxxStorage.removeItem('key')`该方法接受一个键名作为参数，并把该键对应的键值对从存储中删除
+ `xxxStorage.clear()`该方法会清空存储中的所有数据

==备注==

+ `SessionStorage`存储中的内容会随着游览器窗口关闭而消失
+ `localStorage`存储的内容，需要手动清楚才会消失
+ `xxxStorage.getItem(xxx)`如果xxx对应的value获取不到，那么`getItem()`所获取到的返回值为`null`

`localStorage`

```html
<h2>localStorage</h2>
<button onclick="saveDate()">点我保存数据</button><br/>
<button onclick="readDate()">点我读数据</button><br/>
<button onclick="deleteDate()">点我删除数据</button><br/>
<button onclick="deleteAllDate()">点我清空数据</button><br/>

<script>
  let person = {name:"JOJO",age:20}
  function saveDate(){
    localStorage.setItem('msg','localStorage')
    localStorage.setItem('person',JSON.stringify(person))
  }
  function readDate(){
    console.log(localStorage.getItem('msg'))
    const person = localStorage.getItem('person')
    console.log(JSON.parse(person))
  }
  function deleteDate(){
    localStorage.removeItem('msg')
    localStorage.removeItem('person')
  }
  function deleteAllDate(){
    localStorage.clear()
  }
</script>

```

`sessionStorage`

```html
<h2>sessionStorage</h2>
<button onclick="saveDate()">点我保存数据</button><br/>
<button onclick="readDate()">点我读数据</button><br/>
<button onclick="deleteDate()">点我删除数据</button><br/>
<button onclick="deleteAllDate()">点我清空数据</button><br/>
<script>
  let person = {name:"JOJO",age:20}
  function saveDate(){
    sessionStorage.setItem('msg','sessionStorage')
    sessionStorage.setItem('person',JSON.stringify(person))
  }
  function readDate(){
    console.log(sessionStorage.getItem('msg'))
    const person = sessionStorage.getItem('person')
    console.log(JSON.parse(person))
  }
  function deleteDate(){
    sessionStorage.removeItem('msg')
    sessionStorage.removeItem('person')
  }
  function deleteAllDate(){
    sessionStorage.clear()
  }
</script>

```

#### 使用本地存储优化Tood-List

`src/App.Vue`

```vue
<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader :receive="receive" ></MyHeader>
        <List
            :todos="todos"
            :updateTodo="updateTodo"
            :deleteTodo="deleteTodo">
        </List>
        <Footer :todos="todos" :deleteAll="deleteAll"></Footer>
      </div>
    </div>
  </div>
</template>

<script>
import MyHeader from './components/MyHeader';
import List from './components/List';
import Footer from './components/Footer';
export default {
  name: 'App',
  components:{MyHeader,List,Footer},
  data(){
    return{
      todos:JSON.parse(localStorage.getItem('todos')) || []
    }
  },
  methods:{
    //添加 任务
    receive(todo){
      this.todos.unshift(todo)
    },
    //修改任务状态
    updateTodo(id){
      this.todos.forEach((todo)=>{
        if(todo.id === id){
          todo.done = !todo.done
        }
      })
    },
    //删除 单个
    deleteTodo(id){
      this.todos.forEach((todo)=>{
        if(todo.id === id){
            // this.todos.delete()
          // this.todos.
          this.todos = this.todos.filter(t =>{
            return t.id !== id
          })
          console.log("我要删除了")
        }
      })
    },
    //删除完成任务
    deleteAll(){
      if (confirm("确定吗？")) {
        this.todos = this.todos.filter(todo => {
          return !todo.done
        })
      }
    }
  },
  watch:{
    todos:{
      deep:true,
      handler(value){
        localStorage.setItem('todos',JSON.stringify(value))
      }
    }
  }
}
</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

</style>

```

### 组件的自定义事件

1. 一种组件间通信的方式，适用于：子组件 ---> 父组件

2. 使用场景：子组件想给父组件传数据，那么就要在父组件中给子组件绑定自定义事件(事件的回调在A中)

3. 绑定自定义事件

   1. 第一种，在父组件中`Demo @事件名="方法"/` 或`Demo v-on:事件名="方法"/`

   2. 第二种方式，在父组件中`this.$refs.demo.$on('事件名',方法)`

      ```javascript
      <Demo ref="demo"/>
      ......
      mounted(){
         this.$refs.demo.$on('atguigu',this.test)
      }
      ```

   3. 若想让自定义事件只能触发一次，可以使用`once`修饰符，或`$noce`方法

4. 触发自定义事件`this.$emit('事件名',数据)`

5. 接触自定义事件`this.$off('事件名')`

6. 组件上也可以绑定原生`DOM`事件，需要使用`native`修饰符`@click.native="show"`

   上面绑定自定义事件，及时绑定的是原生事件也会被认为是自定义的，需要加上`native`，加了后就将此事件给组件的根元素

7. 注意：通过`this.$refs.xxx.$on('事件名',回调函数)`绑定自定义时间时，回调函数要么配置在`methods`中，要么用箭头函数，否则this指向会出问题

`src/App.Vue`

###全局事件总线(GlobalEventBus)

1. 一种组件间通信的方式，适用于任意组件间通信。

2. 安装全局事件总线

   ```javascript
   new Vue({
       ....
       beforeCreate(){
       	Vue.prototype.$bus = this 
   }
   })
   ```

3. 使用事件总线：

   + 接受数据：A组件想接收数据，则在A组件中给`$bus`绑定自定义事件。时间的回调留在A组件自身。

     ```vue
     methods(){
       demo(data){......}
     }
     ......
     mounted() {
       this.$bus.$on('xxxx',this.demo)
     }
     ```

   + 提供数据：`this.$bus.$emit('xxx',数据)

4. 最好在`beforeDestroy`钩子中，用`$off`去解绑当前组件所用到的事件。

### 消息订阅预发布(pubsub)

1. 一种组件间通信的方式，适用于任意组件间通信【

2. 使用步骤：

   + 安装pubsub：`npm i pubsub-js`

   + 引入：`import pubsub from 'pubsub-js'`

   + 接受数据：A组件想接受数据，则在A组件中订阅消息，订阅的回调函数留在A组件自身。

     ```vue
     methods(){
       demo(data){......}
     }
     ......
     mounted() {
       this.pid = pubsub.subscribe('xxx',this.demo) //订阅消息
     }
     ```

   + 提供数据：`pubsub.publish('xxx',数据)`

   + 最好在`beforeDestroy`钩子中，用`pubsub.unsubscribe`去解绑当前组件所用到的事件。
