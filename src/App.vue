<template>
  <div class="app-container">

  <!-- 使用组件 -->
  <!-- Header区域 -->
  <Header title="购物车案例"></Header>

  <!-- 循环渲染每一个商品信息 Goods -->
  <Goods 
   v-for="item in list"
   :key="item.id"
   :id="item.id"
   :title="item.goods_name"
   :pic="item.goods_img"
   :price="item.goods_price"
   :state="item.goods_state"
   :count="item.goods_count"
   @state-change="getNewState"
  ></Goods> 

  <!-- Footer 组件区域 -->
  <Footer :isFull="fullState" :amount="amt" :all="total" @full-change="getFullState"></Footer>
  </div>

</template>

<script>
// 导入axios请求库
import axios from 'axios'
// 导入头部组件-----Header
import Header from '@/components/Header/Header.vue'
//导入商品信息组件----Goods
import Goods from '@/components/Goods/Goods.vue'
//导入底部结算组件-----Footer
import Footer from '@/components/Footer/Footer.vue'
//导入bus组件
import bus from '@/components/eventBus.js'

export default {

  components: {
    //注册组件
    Header,
    Goods,
    Footer,
  },
  data() {
    return {
      //用来存储购物车列表数据，默认为空数组，请求成功后赋值
      list: []
    }
  },
  computed: {
    //根据列表的每一件商品，动态计算出一个布尔值，将这个值给footer组件中的复选框使用
    //动态计算出全选的状态是true还是false
    fullState() {
      return this.list.every(item => item.goods_state)
    },
    //已勾选商品的总价格
    amt(){
      // 1.先filter过滤
      // 2.再reduce累加
      return this.list
      .filter(item => item.goods_state === true)
      .reduce((total, item) => 
         (total += item.goods_price * item.goods_count), 0)
    },
    //已勾选商品的总数量
    total() {
      return this.list.filter(item => item.goods_state)
      .reduce((t, item) => 
       t += item.goods_count, 0)
    },
  },
  methods: {
    // 封装请求列表数据的方法
    async initCartList() {
      //调用axios的GET方法，请求列表数据
      //调用get返回值是promise，使用await和async修饰
      const { data: res } = await axios.get('https://www.escook.cn/api/cart')
      //只要请求回来的数据在页面渲染期间要用到，就必须转存到data中
      if (res.status === 200) {
        this.list = res.list
      }
    },
    getNewState(e) {
    //接收子组件传递过来的数据
    // e的格式为{id，value}
       this.list.some(item => {
         if(item.id === e.id) {
          item.goods_state = e.value
        //终止后续的循环
          return true
        }
      })
    },
    getFullState(e) {
      //接收footer子组件传递过来的全选按钮的状态
      this.list.forEach(item => (item.goods_state = e))
    },
  },
  created() {
    //调用请求数据的方法，在生命周期函数created中
    this.initCartList()
    //实现数量值的修改
    bus.$on('share', (val) => {
      this.list.some(item => {
        if(item.id === val.id) {
          item.goods_count = val.value
          return true
        }
      })
    }, )
  },
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
