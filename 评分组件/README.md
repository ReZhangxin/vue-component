# 评分组件

## 第一步：分析需求

> 在页面中大量需要用到评分，比如商家评分，骑手评分，美食评分
> 因此很多地方需要，所以要单拿出来。

## 第二步：思路

> 如果评4.5分以下，4分以上全部是4颗星，如果评4.5分以上，5分以下全部是4.5颗星

- 1、判断多少平分：如果评分为3.7分,首先通过计算**向下取0.5**获得分数3.5

- 2、添加完整星星：根据上面评分的整数多少，向数组添加多少星星

- 3、添加半个星星：**判断**是否有半个的星星，如果有就想数组添加，反之不添加

- 4、添加灰色星星：最后由上面2,3步获得的数组长度和5进行比较，不足的话向其中添加灰色星星

## 第三步： 步骤

在`header.vue`组件中使用评分组件

- 第一：在模板中使用评分组件

> 有两个参数分别是大小（**size**）和分数（**score**）

```html
<template>
    <div>
        <star :size="48" :score="seller.score"></star>
    </div>
</template>
```

- 第二：在`<script></script>`标签中引入：

```js
import star from '@/components/star/star'
export default {
    components: {
        star
    }
}
```

### star.vue组件

父组件把两个参数传递给子组件，子组件就应该用`props`接收

```js
export default {
    props: {
        size: {
            type: Number
        },
        score: {
            type: Number
        }
    }
}
```

设置模板

> 首先要有自己的样式加一个class，然后因为在不同的地方大小不一样（比如商家详情页星星比较大，而在评论页星星比较小）这里就会遇到不同的`size`，不同的`size`，样式也会不同。
所以要给其一个动态的class可以在不同大小选择不同样式

```html
<div class="star" :class="starType"></div>
```

通过计算属性返回不同size

> 因为传递给我们的size是一个数值，所以必须转化为相应的class

```js
computed: {
    starType () {
        return 'star-' + this.size
    }
}
```

添加每一个星星

> 一共有5个所以要用for循环

```html
<div class="star" :class="starType">
    <span v-for="item in itemStar" :key="item"
    :class="itemStar" class="star-item"></span>
</div>
```

itemStar是一个数组这个数组就是通过`seller.score`获得的评分来规定数组中有几个黄色的星星。

星星一共有三种状态所以要申请一些常量

```js
const LENGTH = 5
const STR_ON = 'on'
const STR_HALF = 'half'
const STR_OFF = 'off'
```

主要逻辑：

```js
computed: {
    itemStar () {
        const result = [];
        //向下取0.5,0.5以上为0.5 0.5以下为0
        const score = Math.floor(this.score * 2) / 2;
        //向下取整
        const integer = Math.floor(score);
// 上面score只有两种情况0.5或者0，只有不为0的情况下，才可以添加只有一半的星星
        const decimal = score % 1 !== 0;
        for (let i =0; i < integer; i++){
            result.push(CLS_ON)
        }
        if (decimal) {
            result.push(CLS_HALF)
        }
        while (result.length < LENGTH) {
            result.push(CLS_OFF)
        }
        return result
    }
}
```

### 完整代码：

```vue
<template>
  <div class="star" :class="starType">
      <span v-for="(itemClass,index)  in itemClasses"
      :key="itemClass" :class="itemClass" class="star-item"></span>
  </div>
</template>

<script>
const LENGTH = 5
const CLS_ON = 'on'
const CLS_HALF = 'half'
const CLS_OFF = 'off'

export default {
  props: {
    size: {
      type: Number
    },
    score: {
      type: Number
    }
  },
  computed: {
    starType () {
      return 'star-' + this.size
    },
    itemClasses () {
      const result = []
      // 向下取0.5
      const score = Math.floor(this.score * 2) / 2
      const hasDecimal = score % 1 !== 0
      const integer = Math.floor(score)
      for (let i = 0; i < integer; i++) {
        result.push(CLS_ON)
      }
      if (hasDecimal) {
        result.push(CLS_HALF)
      }
      while (result.length < LENGTH) {
        result.push(CLS_OFF)
      }
      return result
    }
  }
}
</script>

<style lang='stylus'>
@import '../../common/stylus/mixin.styl'
  .star
    font-size: 0
    .star-item
      display inline-block
      background-repeat no-repeat
    &.star-48
      .star-item
        width 20px
        height 20px
        margin-right 22px
        background-size 20px 20px
        &.last-child
          margin-right 0
        &.on
          bg-image('star48_on')
        &.half
          bg-image('star48_half')
        &.off
          bg-image('star48_off')
    &.star-36
      .star-item
        width 15px
        height 15px
        margin-right 6px
        background-size 15px 15px
        &.last-child
          margin-right 0
        &.on
          bg-image('star36_on')
        &.half
          bg-image('star36_half')
        &.off
          bg-image('star36_off')
    &.star-24
      .star-item
        width 10px
        height 10px
        margin-right 3px
        background-size 10px 10px
        &.last-child
          margin-right 0
        &.on
          bg-image('star24_on')
        &.half
          bg-image('star24_half')
        &.off
          bg-image('star24_off')

</style>

```

### mixin样式

```css
bg-image($url)
    background-image url($url + '@2x.png')
    @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3)
        background-image url($url + '@3x.png')
```
