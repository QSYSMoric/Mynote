+ 利用属性命令的特性v-bind我们可以绑定任意的元素属性与虚拟的数据绑定，对于特殊的属性style与class，vue3.0提供多种方式
### 1、绑定对象
+ 我们可以给 `:class` (`v-bind:class` 的缩写) 传递一个对象来动态切换 class：
+ 在对象中，key代表class选择规则，value为布尔型，true代表应用，false代表不应用
```html
<div :class="{ active: isActive }"></div>
```
---
```js
const isActive = ref(true)
const hasError = ref(false)
```
---
```js
<div
  class="static"
  :class="{ active: isActive, 'text-danger': hasError }"
></div>

const classObject = reactive({
  active: true,
  'text-danger': false
})
```
+ 那么结果为：
```js
<div class="static active"></div>
```
### 2、绑定数组
+ 我们可以给:class绑定一个多数组来渲染多个css ruler：
```js
<div :class="[activeClass, errorClass]"></div>
```
+ 如果想要有条件的渲染某个css则可以使用三元运算符
```js
<div :class="[isActive ? activeClass : '', errorClass]"></div>
```
### 3、在组件上使用绑定class
+ 对于只有<font>1个根元素</font>的组件，当使用了class attribute这些class元素会被添加到组件的根元素的class上
```js
<!-- 子组件模板 -->
<p class="foo bar">Hi!</p>
<!-- 在使用组件时 -->
<MyComponent class="baz boo" />
//所渲染出来的便是
<p class="foo bar baz boo">Hi!</p>
```
### 4、绑定内联样式style
#### 4.1、\:style='{object}',支持绑定一个对象值
+ 对象值的属性即为style的属性
+ 尽管推荐使用 camelCase，但 `:style` 也支持 kebab-cased 形式的 CSS 属性 key (对应其 CSS 中的实际名称)，例如：
```js
<div :style="{ 
'font-size': fontSize + 'px' 
}"></div>
```
#### 4.2、绑定数组
+ 我们还可以给 `:style` 绑定一个包含多个样式对象的数组。这些对象会被合并后渲染到同一元素上：
```js
<div :style="[baseStyles, overridingStyles]"></div>
```
### 4.3、自动前缀
+ 当你在 `:style` 中使用了需要[浏览器特殊前缀](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix)的 CSS 属性时，Vue 会自动为他们加上相应的前缀。Vue 是在运行时检查该属性是否支持在当前浏览器中使用。如果浏览器不支持某个属性，那么将尝试加上各个浏览器特殊前缀，以找到哪一个是被支持的。
### 4.4、样式多值
+ 可以对一个样式属性提供多个不同前缀的值、例如，
```js
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```
