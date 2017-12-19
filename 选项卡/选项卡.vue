<template>
  <div class="s-tab">
    <ul class="title-list">
      <li class="bottom" ref="bottom"></li>
      <li @click="tab(index)" class="tab-title" v-for="(info,index) in tabInfo" :key="index" >{{info.title}}</li>
    </ul>
    <ul class="content-list">
      <li class="tab-content" v-for="(info,index) in tabInfo" :key="index" v-show="index === num">{{info.content}}</li>
    </ul> 
  </div>
</template>

<script>
export default {
  data () {
    return {
      tabInfo: [
        { title: '用户管理', content: '用户管理~' },
        { title: '配置管理', content: '配置管理~' },
        { title: '角色管理', content: '角色管理~' }
      ],
      num: 1
    }
  },
  methods: {
    tab: function (index) {
      this.num = index
      console.log(this.$refs.bottom)
      animate(this.$refs.bottom, index * 102)
    }
  }
}
function animate (el, target) {
  clearInterval(el.timer)
  el.timer = setInterval(function () {
    let step = (target - el.offsetLeft) / 10
    step = step > 0 ? Math.ceil(step) : Math.floor(step)
    el.style.left = el.offsetLeft + step + 'px'
    if (Math.abs(target - el.offsetLeft) <= Math.abs(step)) {
      el.style.left = target + 'px'
      clearInterval(el.timer)
    }
  }, 1000 / 60)
}
</script>

<style lang="stylus" scoped>
.s-tab
  width 90%
  height 200px
  margin 10px auto
  padding 12px
  border 1px solid #ddd
  .title-list
    position relative
    width 100%
    border-bottom 2px solid #E4E7ED
    font-size 0
    color #7e8c8d
    .tab-title
      display inline-block
      width 90px
      padding 10px 0
      margin-right 12px
      font-size 14px
      &:last-child
        margin-right 0
    .bottom
      position absolute
      bottom -1px
      left 0
      height 2px
      width 60px
      background #409eff
  .content-list
    width 100%
    padding 12px 0
</style>