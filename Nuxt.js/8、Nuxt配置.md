## 1、环境覆盖
+ 环境覆盖
```js
export default defineNuxtConfig({
  $production: {
    routeRules: {
      '/**': { isr: true }
    }
  },
  $development: {
    //
  }
});
```
+ 用于定义生产环境与开发环境，针对不同环境下，区分配置
## 2、环境变量与私有令牌
+ runtimeConfig将环境变量这样的值暴露给应用程序的其余部分，默认情况下，这些键只能在服务端访问，但是runtimeConfig.public中的键也可也在客户端中使用
```js
  runtimeConfig:{
    apiSercret:"123",
    public:{
      apiBase:"/api"
    }
  }
```
```env
# 这将覆盖apiSecret的值
NUXT_API_SECRET=api_secret_token
```
+ 这些值应该在`nuxt.config`中定义，并可以使用环境变量进行覆盖。
+ 这些变量通过`useRuntimeConfig()`组合函数暴露给应用程序的其余部分。
```js
<script setup lang="ts">
	const runtimeConfig = useRuntimeConfig()
</script>
```
## 3、应用程序配置
+ `app.config.ts`文件位于源目录中（默认为项目的根目录），用于公开在构建时确定的公共变量。与`runtimeConfig`选项不同，这些变量不能使用环境变量进行覆盖。
+ 一个最简配置文件导出了`defineAppConfig`函数，函数中包含了一个配置对象。`defineAppConfig`助手函数在全局范围内无需导入即可使用。
```js
//app.config.ts
export default defineAppConfig({
    title: 'Hello Nuxt',
    theme: {
      dark: true,
      colors: {
        primary: '#ff0000'
      }
    }
});
```
+ 这些变量通过[`useAppConfig`](https://nuxt.com.cn/docs/api/composables/use-app-config)组合函数暴露给应用程序的其余部分。
```js
    const appConfig = useAppConfig()
    console.log(appConfig);
```
+ 需要注意的是`app.config.ts`文件中的变量会在构建过程中注入到客户端代码中，并成为可在浏览器中访问的全局变量。这样做的目的是方便在客户端代码中使用这些配置变量。
## 4、runtimeConfig与app.config
- `runtimeConfig`：需要在构建后使用环境变量指定的私有或公共令牌。
- `app.config`：在构建时确定的公共令牌，网站配置（如主题变体、标题）以及不敏感的项目配置等。

| 功能           | `runtimeConfig` | `app.config` |
| -------------- | --------------- | ------------ |
| 客户端端       | 已注入          | 已打包       |
| 环境变量       | ✅ 是           | ❌ 否        |
| 响应式         | ✅ 是           | ✅ 是        |
| 类型支持       | ✅ 部分         | ✅ 是        |
| 每个请求的配置 | ❌ 否           | ✅ 是        |
| 热模块替换     | ❌ 否           | ✅ 是        |
| 非原始JS类型   | ❌ 否           | ✅ 是        |
## 5、外部配置文件
+ Nuxt使用`nuxt.config.ts`文件作为配置的唯一来源，并跳过读取外部配置文件。在构建项目的过程中，你可能需要配置这些文件。下表列出了常见的配置以及如何在Nuxt中配置它们（如果适用）。

| 名称                               | 配置文件                | 如何配置                                                                             |
| ---------------------------------- | ----------------------- | ------------------------------------------------------------------------------------ |
| [Nitro](https://nitro.unjs.io/)    | ~~`nitro.config.ts`~~   | 在`nuxt.config`中使用[`nitro`](https://nuxt.com.cn/docs/api/nuxt-config#nitro)键     |
| [PostCSS](https://postcss.org/)    | ~~`postcss.config.js`~~ | 在`nuxt.config`中使用[`postcss`](https://nuxt.com.cn/docs/api/nuxt-config#postcss)键 |
| [Vite](https://vitejs.dev/)        | ~~`vite.config.ts`~~    | 在`nuxt.config`中使用[`vite`](https://nuxt.com.cn/docs/api/nuxt-config#vite)键       |
| [webpack](https://webpack.js.org/) | ~~`webpack.config.ts`~~ | 在`nuxt.config`中使用`webpack`键                                                     |
## 6、Vue配置
### 6.1、使用 Vite
+ 如果你需要传递选项给 `@vitejs/plugin-vue` 或 `@vitejs/plugin-vue-jsx`，你可以在你的 `nuxt.config` 文件中进行配置。
```js
export default defineNuxtConfig({
  vite: {
    vue: {
      customElement: true
    },
    vueJsx: {
      mergeProps: true
    }
  }
});
```
### 6.2、使用 webpack
+ 如果你使用 webpack 并且需要配置 `vue-loader`，你可以在 `nuxt.config` 文件中使用 `webpack.loaders.vue` 键进行配置。可用选项在这里定义。
```js
export default defineNuxtConfig({
  webpack: {
    loaders: {
      vue: {
        hotReload: true,
      }
    }
  }
});
```
