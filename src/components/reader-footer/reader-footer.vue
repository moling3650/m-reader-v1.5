<template>
  <div class="reader-footer" v-show="barShow">
    <div class="footer-bar">
      <div class="top">
        <a class="item prev-chapter"></a>
        <a class="item current-chapter"></a>
        <a class="item next-chapter"></a>
      </div>
      <div class="bottom">
        <a class="item toc"></a>
        <a class="item font" :class="{'on': fontBarShow}" @click.prevent="toggleFontBar"></a>
        <a class="item" :class="nightMode ? 'day' : 'night'" @click.prevent="switchMode"></a>
        <a class="item download"></a>
      </div>
    </div>
    <div class="font-bar" v-show="fontBarShow">
      <div class="font-size">
        <a class="font-large"></a>
        <a class="font-small"></a>
      </div>
      <div class="font-bg">
        <a class="bg-option" v-for="obj in bgStyleObjs" :style="obj"></a>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import { storageSetter } from 'common/js/utils.js'
  export default {
    props: {
      barShow: Boolean,
      bgStyleObjs: Array,
      nightMode: Boolean
    },
    methods: {
      toggleFontBar () {
        this.fontBarShow = !this.fontBarShow
      },
      switchMode () {
        this.nightMode = !this.nightMode
        storageSetter('night-mode', this.nightMode)
      }
    },
    events: {
      'hide-bar' () {
        this.fontBarShow = false
      }
    },
    data () {
      return {
        fontBarShow: false
      }
    }
  }
</script>

<style lang="stylus" scoped>
  @import '../../common/stylus/minin'

  .reader-footer
    position fixed
    right 0
    bottom 0
    left 0
    z-index 2
    background rgba(0, 0, 0, .9)

  .footer-bar
    font-size 0
    color #fff
    .item
      display inline-block
      width 25%
      font-size 10px
      text-align center
      cursor pointer
    .top
      height 48px
      border-bottom 1px solid #333
      line-height 48px
      .prev-chapter:after
        content '上一章'
      .next-chapter:after
        content '下一章'
      .current-chapter
        width 50%
    .bottom
      .item
        position relative
        height 70px
        &:before
          content ''
          position absolute
          bottom 33px
        &:after
          display inline-block
          margin-top 45px
      .toc
        &:before
          margin-left 1px
          bg-img('toc.png', 18px, 14px)
        &:after
          content '目录'
      .font
        &:before
          bg-img('font.png', 20px, 13px)
        &:after
          content '字体'
        &.on:before
          margin-bottom -1px
          margin-left -1px
          bg-img('font_on.png', 22px, 15px)
      .day
        &:before
          margin-left 1px
          bg-img('day.png', 18px, 18px)
        &:after
          content '白天'
      .night
        &:before
          margin-left 2px
          bg-img('night.png', 16px, 16px)
        &:after
          content '夜间'
      .download
        &:before
          bg-img('download.png', 22px, 16px)
        &:after
          content '下载'

  .font-bar
    position fixed
    right 0
    bottom 70px
    left 0
    z-index 2
    height 135px
    color #fff
    background rgba(0, 0, 0, .9)

  .font-size, .font-bg
    margin-top 15px
    padding 5px 10px
    &:before
      margin-right 20px
      margin-left 10px
      font-size 14px
      line-height 21px
  .font-size:before
    content '字号'
  .font-bg:before
    content '背景'

  .font-large, .font-small
    display inline-block
    padding 5px 40px
    border 1px solid #8c8c8c
    border-radius 16px
    margin 0 5px
    font-size 13px
    line-height 16px
  .font-large:before
    content '大'
  .font-small:before
    content '小'

  .bg-option
    position relative
    display inline-block
    width 30px
    height 30px
    border-radius 50%
    margin-right 10px
    vertical-align middle
    &.on:after
      content ''
      position absolute
      top -2px
      left -2px
      border 1px solid #ff7800
      border-radius 50%
      width 32px
      height 32px
</style>
