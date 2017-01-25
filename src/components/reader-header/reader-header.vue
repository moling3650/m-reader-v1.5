<template>
  <div class="reader-header">
    <a class="back"></a>
    <a class="more"></a>
    <div class="popup">
      <div class="book">
        <img class="cover" :src="book.cover" :alt="book.title" />
        <h1 class="title">{{ book.title }}</h1>
        <p class="author">{{ book.authors }}</p>
      </div>
      <div class="auto-pay"></div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import { getApiData } from 'common/js/utils.js'

  export default {
    data () {
      return {
        book: {}
      }
    },
    ready () {
      getApiData('/api/info', data => {
        this.book = data.items[0]
      })
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
    padding 25px
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
      &.check-on:before
        bg-img('check_on.png', 18px, 18px)
      &:after
        content '自动购买下一章'
        margin-left 34px
        font-size 14px
</style>
