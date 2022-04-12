# 购物车 Vue2.0

这是一个经典的购物车功能项目，包含了以下功能：

- 使用 axios 的 GET 方法，请求数据列表
- 封装所需要的组件，提高组件复用
- 使用 v-for 循环渲染商品的信息
- 各组件之间数据共享，props 动态渲染属性
- 使用自定义事件修改商品的复选框勾选状态以及各组件之间的动态传值
- 使用计算属性 computed 定义全选状态并且将全选的值返回给每一个商品
- computed 方法总价格和结算数量
- eventBus 方法实现修改商品数量

## Project setup

```
npm install
```

### Compiles and hot-reloads for development

```
npm run serve
```

### Compiles and minifies for production

```
npm run build
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).
