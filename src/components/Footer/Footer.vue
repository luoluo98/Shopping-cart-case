<template>
  <div class="footer-container">
    <!-- 左侧的全选 -->
    <div class="custom-control custom-checkbox">
      <input type="checkbox" class="custom-control-input" id="cbFull" :checked="isFull" @change="fullChange"/>
      <label class="custom-control-label" for="cbFull">全选</label>
    </div>

    <!-- 中间的合计 -->
    <div>
      <span>合计：</span>
      <!-- toFixed(2) 价格小数点后面的位数 -->
      <span class="total-price">￥{{ amount.toFixed(2) }}</span>
    </div>

    <!-- 结算按钮 -->
    <button type="button" class="btn btn-primary btn-settle">结算（{{ all }}）</button>
  </div>
</template>

<script>
export default {
  props: {
    //全选的状态
    isFull: {
      default: true,
      type: Boolean
    },
    //总价格
    amount: {
      default: 0,
      type: Number
    },
    //已勾选的商品的总数量
    all: {
      default: 0,
      type: Number
    },
  },
  methods: {
    //监听到全选状态的变化，拿到状态后，将没意见商品的勾选状态改为true，不用传id
    fullChange(e) {
      this.$emit('full-change', e.target.checked)
    },
  },
}
</script>

<style lang="less" scoped>
.footer-container {
  font-size: 12px;
  height: 50px;
  width: 100%;
  border-top: 1px solid #efefef;
  position: fixed;
  bottom: 0;
  background-color: #fff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10px;
}

.custom-checkbox {
  display: flex;
  align-items: center;
}

#cbFull {
  margin-right: 5px;
}

.btn-settle {
  height: 80%;
  min-width: 110px;
  border-radius: 25px;
  font-size: 12px;
}

.total-price {
  font-weight: bold;
  font-size: 14px;
  color: red;
}
</style>
