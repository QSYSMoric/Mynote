 ## 1、中间件
+ Nuxt提供了一个可定制的路由中间件，用于在客户端不同页面导航的时候验证并取消导航
## 2、中间件的类型
+ 匿名(内联)路由中间件，直接在组件或页面中定义
+ 命名路由中间件，放置在middleware/目录下，并且在页面上使用时通过异步导入自动加载
+ 全局路由中间件，放置在middleware/目录下，并且以.global后缀结尾，在每次路由更改时运行
+ 前两种类型的路由中间件可以在definePageMeta中定义
## 3、使用方法
+ 路由中间件就是导航守卫，接收当前路路由和下一个路由作为参数
```js
export default defineNuxtRouteMiddleware((to,from)=>{
    console.log(to);
    console.log(from);
});
```
+ 与vue-router不同的是，中间件不提供第三个参数next( )，重定向或取消路由从中间件返回值处理
+ 根据返回值不同，中间件的操作也不同：
	+ 无，不阻止导航，将继续执行下一个中间函数
	+ return navigateTo('/')：重定向到给定的路径，并且在服务器端发送路由时候设置状态码为302
	+ return navigateTo('/',{redirectCode:301})：重定向到指定路径，并重设状态码为301
	+ return abortNavigation()：停止当前导航
	+ return abortNavigation(error)：拒绝当前导航并提供错误信息
## 4、中间件执行顺序
+ 中间件按照以下顺序执行：1、全局中间件 > 2、页面定义中间件顺序
```js
middleware|
--| analytics.globa.ts
--| setup.globa.ts
--| auth.js
```
```js
<script setup lang="ts">
definePageMeta({
  middleware: [
    function (to, from) {
      // 自定义内联中间件
    },
    'auth',
  ],
});
</script>
```
+ 可以期望中间件按照以下顺序运行
```js
1、analytics.global.ts
2、setup.global.ts
3、自定义内联中间件
4、auth.ts
```
## 5、指定全局中间件顺序
+ 我们可以在middleware中以特定的“字母编号”
```js
middleware/
--| 01.setup.global.js
--| 02.analytics.global.js
--| auth.js
```
+ 文件名是按照字符串排序的并不是按照数值排序
