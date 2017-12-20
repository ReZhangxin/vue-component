<template>
  <div class="cartcontrol">
    <transition name="slide-fade">
      <div class="cart-decrease icon-remove_circle_outline" 
      v-show="food.count>0" @click="decreaseCart"></div>
    </transition>
    <transition name="slide-fade">
      <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    </transition>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>

<script>
import Vue from 'vue'

export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    addCart () {
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
      } else {
        this.food.count ++
      }
    },
    decreaseCart () {
      this.food.count --
    }
  }
}
</script>

<style lang="stylus">
.cartcontrol
  font-size 0
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

.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to {
  transform: translateX(10px);
  opacity: 0;
}
</style>
