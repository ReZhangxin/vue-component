# 评分组件

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

## star.vue组件

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

> 如果评4.5分以下，4分以上全部是4颗星，如果评4.5分以上，5分以下全部是4.5颗星

**思路:**

- 1、如果评分为3.7分,首先通过计算**向下取0.5**获得分数3.5

- 2、添加完整星星：根据上面评分的整数多少，向数组添加多少星星

- 3、添加半个星星：**判断**是否有半个的星星，如果有就想数组添加，反之不添加

- 4、添加灰色星星：最后由上面2,3步获得的数组长度和5进行比较，不足的话向其中添加灰色星星

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