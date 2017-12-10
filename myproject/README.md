# myproject

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run e2e tests
npm run e2e

# run all tests
npm test
```


For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

## vue 模板语法

~~~规则： 
	--Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染
	  Vue 为你提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的 key 属性即可：
		<template v-if="loginType === 'username'">
		  <label>Username</label>
		  <input placeholder="Enter your username" key="username-input">
		</template>
		<template v-else>
		  <label>Email</label>
		  <input placeholder="Enter your email address" key="email-input">
		</template>
	--
	{{msg}} :文本插值
	v-once  : 一次渲染，不会因数据改变而变化(注意会影响到该节点上的所有数据绑定)
			<span v-once>这个将不会改变: {{ msg }}</span>

	v-html  : 输出html代码和文本(双大括号会将数据解释为普通文本，而非 HTML 代码)
			<p>Using mustaches: {{ rawHtml }}</p>
			<p>Using v-html directive: <span v-html="rawHtml"></span></p>

	v-if    : 指令将根据表达式 seen 的值的真假来加载DOM节点
	v-else  : 指令来表示 v-if 的“else 块”
   v-else-if: 也必须紧跟在带 v-if 或者 v-else-if 的元素之后

    v-show  : 根据条件展示元素(简单地切换元素的 CSS 属性 display)
    		  !!注意，v-show 不支持 <template> 元素，也不支持 v-else。

    v-if vs v-show

		v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
		v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
		v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
		一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。
		因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

	v-for  :指令根据一组 数组 的选项列表进行渲染
			需要使用 item in items 形式的特殊语法，items 是源数据数组 并且 item 是数组元素迭代的别名。
				当有两个参数时{item，index}第一个为内容，第二个为索引

			也可以迭代一个对象 value in object ，value属性值 object对象
				两个参数{value,key} {值，键}
				三个参数{value,key，index} {值，键，索引}

			！！在遍历对象时，是按 Object.keys() 的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下是一致的。



	v-if & v-for
			  当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级。

	v-bind  : 缩写 v-bind:href="url" ==>  :href="url"
			  1.v-bind:href="url"  href 是参数，告知 v-bind 指令将该元素的 href 属性与表达式 url 的值绑定。
			  2.<div v-bind:class="{ active: isActive }"></div>
			  	表示 active 这个 class 存在与否将取决于数据属性 isActive 的 truthiness
			  3.<div class="static"
     			 v-bind:class="{ active: isActive, 'text-danger': hasError }">
				</div>
				data: {
				  isActive: true,
				  hasError: false
				}
				结果渲染为：
				<div class="static active"></div>

	v-on    :   事件监听 :click = "function" 函数名
				缩写 v-on:click="fun" ==>  @click="fun"
				v-on:click = "code"//也可以写js代码eg:"count+=1"
				事件修饰符:
				.stop
				.prevent
				.capture
				.self
				.once
				<!-- 阻止单击事件继续传播 -->
				<a v-on:click.stop="doThis"></a>

				<!-- 提交事件不再重载页面 -->
				<form v-on:submit.prevent="onSubmit"></form>

				<!-- 修饰符可以串联 -->
				<a v-on:click.stop.prevent="doThat"></a>

				<!-- 只有修饰符 -->
				<form v-on:submit.prevent></form>

				<!-- 添加事件监听器时使用事件捕获模式 -->
				<!-- 即内部元素触发的事件先在此处处理，然后才交由内部元素自身进行处理 -->
				<div v-on:click.capture="doThis">...</div>

				<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
				<!-- 即事件不是从内部元素触发的 -->
				<div v-on:click.self="doThat">...</div>

				!!!!!使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 @click.prevent.self 会阻止所有的点击，而 @click.self.prevent 只会阻止对元素自身的点击。

				!新增<!-- 点击事件将只会触发一次 -->
					<a v-on:click.once="doThis"></a>
	修饰符  ：(Modifiers) 是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。
			  例如，.prevent 修饰符告诉 v-on 	指令对于触发的事件调用 event.preventDefault()：
				<form v-on:submit.prevent="onSubmit">...</form>
	数组的变异方法和非变异方法 : 变异方法(mutation method)会改变被这些方法调用的原始数组

	  #注意事项

		由于 JavaScript 的限制，Vue 不能检测以下变动的数组：

		当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
		当你修改数组的长度时，例如：vm.items.length = newLength

		为了解决第一类问题，以下两种方式都可以实现和 vm.items[indexOfItem] = newValue 相同的效果，同时也将触发状态更新：
		// Vue.set
		Vue.set(example1.items, indexOfItem, newValue)
		// Array.prototype.splice
		example1.items.splice(indexOfItem, 1, newValue)

		为了解决第二类问题，你可以使用 splice：
		example1.items.splice(newLength)

~~~

~~~QUESTION
	1.Mustache 语法不能作用在 HTML 特性上，遇到这种情况应该使用 v-bind 指令：？？
	什么是不能作用？
	2.再看看官方文档的  列表渲染/一个组件的v-for
	3.vue.component()
~~~
