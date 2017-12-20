<template>
  <div>
    <ul>
      <li class="foods" v-for="(food,index) in foods" :key="index">
        <img :src="food.icon" alt="233~" width=50>
        <span>{{food.name}}</span>
        <span class="red">￥{{food.price}} 元</span>
        <div class="cartcontrol-wrapper">
          <cartcon :food="food"></cartcon>
        </div>
        
      </li>
    </ul>
    <div class="shopcart">
      <span>总价格为：{{totalPrice}}元</span>
      <span>总数为：{{totalCount}}</span>
    </div>
  </div>
</template>

<script>
import cartcon from '@/components/seller/cartcon'
export default {
  data () {
    return {
      foods: [
        { name: '皮蛋瘦肉粥1', price: 11, icon: 'http://fuss10.elemecdn.com/c/cd/c12745ed8a5171e13b427dbc39401jpeg.jpeg?imageView2/1/w/114/h/114' },
        { name: '皮蛋瘦肉粥2', price: 12, icon: 'http://fuss10.elemecdn.com/c/cd/c12745ed8a5171e13b427dbc39401jpeg.jpeg?imageView2/1/w/114/h/114' },
        { name: '皮蛋瘦肉粥3', price: 13, icon: 'http://fuss10.elemecdn.com/c/cd/c12745ed8a5171e13b427dbc39401jpeg.jpeg?imageView2/1/w/114/h/114' }
      ]
    }
  },
  computed: {
    totalPrice () {
      let total = 0
      this.foods.forEach(food => {
        if (food.count) {
          total += food.price * food.count
        }
      })
      return total
    },
    totalCount () {
      let count = 0
      this.foods.forEach(food => {
        if (food.count) {
          count += food.count
        }
      })
      return count
    }
  },
  components: {
    cartcon
  }
}
</script>

<style lang="stylus" scoped>
.foods
  position relative
  width 100%
  height 50px
  padding 12px 12px 0
  color #93999f
  img
    vertical-align top
  span 
    display inline-block
    vertical-align top
    line-height 50px
  .red
    color coral
.cartcontrol
  font-size 0
  position absolute
  right 30px
  top 19px
  .cart-decrease,.cart-add
    display inline-block
    line-height 24px
    font-size 24px
    padding 6px
    color rgb(0,160,220)
  .cart-count
    display inline-block
    vertical-align top
    width 12px
    padding-top 6px
    font-size 10px
    line-height 24px
    text-align center
    color rgb(147,153,159)
.cartcontrol-wrapper
  position absolute
  width 150px
  bottom 62px
  right 0
</style>