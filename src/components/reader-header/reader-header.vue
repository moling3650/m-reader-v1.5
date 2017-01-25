<template>
  <div class="reader-header" v-show="barShow">
    <a class="back" @click="goBack"></a>
    <a class="more" @click.prevent="togglePopupShow"></a>
    <div class="popup" v-show="popupShow">
      <div class="book" @click="goBack">
        <img class="cover" :src="book.cover" :alt="book.title" />
        <h1 class="title">{{ book.title }}</h1>
        <p class="author">{{ book.authors }}</p>
      </div>
      <div class="auto-pay" :class="{'on': isAutoBuy}" @click="toggleAutoBuy"></div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import { storageGetter, storageSetter } from 'common/js/utils.js'
  export default {
    props: {
      barShow: Boolean,
      book: Object
    },
    methods: {
      togglePopupShow () {
        this.popupShow = !this.popupShow
      },
      toggleAutoBuy () {
        this.isAutoBuy = !this.isAutoBuy
        storageSetter('auto_buy', this.isAutoBuy)
      },
      goBack () {
        window.alert('回到书籍详情页面')
      }
    },
    events: {
      'hide-bar' () {
        this.popupShow = false
      }
    },
    data () {
      return {
        popupShow: false,
        isAutoBuy: false
      }
    },
    ready () {
      this.isAutoBuy = storageGetter('auto_buy') === 'true'
    }
  }
</script>

<style lang="stylus" scoped>
  @import '../../common/stylus/minin'

  .reader-header
    position fixed
    top 0
    right 0
    left 0
    z-index 2
    height 50px
    font-size 14px
    line-height 50px
    color #d5d5d6
    background rgba(0, 0, 0, .9)

  .back
    display inline-block
    padding 25px 50px
    cursor pointer
    &:before
      content ''
      position absolute
      top 13px
      left 13px
      bg-img('back.png', 24px, 24px)
    &:after
      content '返回'
      position absolute
      top 0px
      left 50px

  .more
    position absolute
    right 0
    padding 25px 50px
    cursor pointer
    &:before
      content ''
      position absolute
      right 38px
      margin-top -3px
      bg-img('more.png', 24px, 6px)

  .popup
    position absolute
    right 25px
    display block
    width 182px
    border-radius 6px
    background rgba(0, 0, 0, .85)
    &:before
      content ''
      position relative
      top -8px
      left 150px
      display block
      width 0
      height 0
      border 8px solid transparent
      border-top 0
      border-bottom-color rgba(0, 0, 0, .85)
    .book
      min-height 74px
      padding 12px 10px
      overflow hidden
      cursor pointer
      .cover
        position absolute
        width 54px
        height 72px
      .title
        margin-top 14px
        margin-left 64px
        font-size 15px
        font-weight bold
        line-height 1
        color #ddd
      .author
        margin-left 62px
        font-size 12px
        line-height 28px
        color #9d9d9c
    .auto-pay
      border-top 1px solid #5e5e5e
      cursor pointer
      &:before
        content ''
        position absolute
        margin 16px 10px
        bg-img('check.png', 18px, 18px)
      &.on:before
        bg-img('check_on.png', 18px, 18px)
      &:after
        content '自动购买下一章'
        margin-left 34px
        font-size 14px
</style>
