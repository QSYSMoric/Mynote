## 1、简介
+ 类似于Android的layout，nuxt提供一个layouts文件夹，用于预定义页面模板，也就是将一个项目中最常用的组件套用在该模板组件中，
+ 当其他组件需要这个布局文件的时候那么，这个布局文件中使用slot插槽将会发挥作用
+ 组件使用布局模板
```js
  <NuxtLayout name="defaut">
    <div> 
      demo
    </div>
  </NuxtLayout>
```
## 2、多插槽的使用
+ 在布局文件中我们可以定义多个插槽，也就是说我们可以准备多个区域，我们将这些插槽命名那么在使用时可以将内容放在指定插槽中去
```js
<template>
  <div>
    <nav><div>默认布局</div></nav>
    <slot></slot>
    <slot name="one"/>
    <slot name="two"></slot>
  </div>
</template>
```
+ 而布局文件的使用
```js
<template>
  <NuxtLayout name="defaut">
    <div> 
      demo
    </div>
    <template #one>
      你好
    </template>
  </NuxtLayout>
</template>
```