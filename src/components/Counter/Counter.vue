<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ num }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
import bus from '@/components/eventBus.js'

export default {
  props: {
    //接收商品的id值，将来使用eventBus方案，把数量传递到app的时候，需要通知app组件更新哪一个商品的数量
    id: {
      require: true,
      type: Number
    },
    //接收Goods传过来的商品数量值num
    num: {
      default: 1,
      type: Number
    },
  },
  methods: {
    //点击按钮，数值加一
    add() {
      //要发送给app的数据格式为{id，value}，其中id是商品的id，value是商品最新的购买数量
      const obj = {id: this.id, value: this.num + 1}
      //通过eventBus把obj对象发送给APP
      bus.$emit('share', obj)
    },
    //点击按钮，数量减一
    sub() {
      if(this.num - 1 === 0) return
      const obj = {id: this.id, value: this.num - 1}
      bus.$emit('share', obj)
    }
  },
}
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
