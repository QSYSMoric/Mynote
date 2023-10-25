### 1、语法
+ 事件处理是一种指令语法绑定
	+ 使用v-on:(事件)="(处理函数)"或者@xxx="function( )"
+ 事件的回调函数需要在methods中配置相应的function函数
+ methods中的this指向绑定的标签
+ function( )可以传参也可以不用传参function，使用参数后若不传$event，那么没有事件对象
```js
<div id="root">
	<a href="httf://www.bilibili.com" 
	@click.prevent="showInFo">点击</a>
</div>
<script>
	new Vue({
		el:"#root",
		methods: {
			showInFo(){
				alert("你好");
			}
		},
	});
</script>
```
### 2、事件修饰符
1. prevent：阻止默认事件
2. stop：阻止事件冒泡
3. once：事件值触发一次
4. capture：使用事件捕获形式
5. self：只有event.target为当前操作的元素才会触发
6. passive：事件默认行为立即执行，无需等待回调函数执行完毕（异步编程）
### 3、键盘事件
+ 属于事件的一种
+ 常见键盘事件
	1. 回车：enter
	2. 删除：delete
	3. 退出：esc
	4. 空格：space
	5. 换行：tab（配合keydown使用）
	6. 上：up
	7. 下：down
	8. 左：left
	9. 右：right
+ vue未提供别名的按键，可以使用案件原始的key值去绑定，但要注意转换为kebek-case
+ 系统修饰键（用法特殊）
	+ 配合keyup使用：按下修饰键的同时，再按下其他键，然后释放其他键，事件才会触发
	+ 配合keydown使用：正常触发事件
+ Vue.config.keyCodes.huiche = 13; //将回车键修饰为huiche
```js
<div id="root">
	<input type="text" @keyup.enter="showInfo">
</div>
<script>
	new Vue({
		el:"#root",
		data() {
			return {
				name:"你看到了我"
			}
		},
		methods: {
			showInfo(){
				alert(this.name);
			}
		},
	});
</script>
```
+ 