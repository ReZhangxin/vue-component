# 购物车

![shopcart](http://p17big5q8.bkt.clouddn.com/x-shopcart.gif)

- [获取count](#获取count)
- [获取总金额](#获取总金额)

本质：就是对选择商品的一种映射，就是选择多少商品。

可以定义一个`selectFoods` 里面包含商品的**数量**

也就是说在原来的数据的基础上再加一个属性count

```js
export default {
    data () {
        return {
            foods: [
                { name: '皮蛋瘦肉粥1', price: 11, icon: 'url1' },
                { name: '皮蛋瘦肉粥2', price: 12, icon: 'url2' },
                { name: '皮蛋瘦肉粥3', price: 13, icon: 'url3' },
                { name: '皮蛋瘦肉粥4', price: 14, icon: 'url4' },
                { name: '皮蛋瘦肉粥5', price: 15, icon: 'url5' }
            ]
        }
    }
}
```

这是dada数据我们需要在goods中加入一个`count`属性

通过另一个小组件来添加`count`

## 获取count

```html
<template>
  <div class="cartcontrol">
    <!-- 左侧减减count -->
    <div class="cart-decrease icon-remove_circle_outline"
    v-show="food.count>0" @click="decreaseCart"></div>
    <!-- 显示count -->
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <!-- 右侧加加count -->
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>
```

此组件中的`food`是foods通过props传值`food`获取的

先实现添加count

```js
addCart () {
    //第一次添加的时候判断是否存在如果不存在设置count为1否则++
    if (!this.food.count) {
        this.food.count = 1
    }
}
```

以上代码无法生效是因为vue的特性，你在给外部对象添加一个不存在的属性是无法完成的，需要用到`Vue.set()`全局API

```js
addCart () {
    //第一次添加的时候判断是否存在如果不存在设置count为1否则++
    if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
    } else {
        this.food.count ++
    }
}
```

[↑ 返回Top](#购物车)

## 获取总金额

总金额的获取（数值，所以需要返回一个数值）

```js
computed: {
    totalPrice () {
        let total = 0
        this.foods.forEach(food => {
        // 因为是异步所以要先判断是否存在food.count,
        // 然后等待dom重新渲染添加count后再计算
            if (food.count){
                total += food.price * food.count
            }
        })
        return total
    }
}
```

[↑ 返回Top](#购物车)