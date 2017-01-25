<template>
  <div class="reader-footer">
    <div class="footer-bar">
      <div class="top">
        <a class="item prev-chapter"></a>
        <a class="item current-chapter"></a>
        <a class="item next-chapter"></a>
      </div>
      <div class="bottom">
        <a class="item toc"></a>
        <a class="item font"></a>
        <a class="item day"></a>
        <a class="item download"></a>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {}
</script>

<style lang="stylus" scoped>
  @import '../../common/stylus/minin'

  .reader-footer
    position absolute
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
          top 20px
        &:after
          display inline-block
          margin-top 45px
      .toc
        &:before
          margin-left 3px
          bg-img('toc.png', 18px, 14px)
        &:after
          content '目录'
      .font
        &:before
          margin-left 2px
          bg-img('font.png', 20px, 13px)
        &:after
          content '字体'
        &.on:before
          margin-top -1px
          margin-left 1px
          bg-img('font_on.png', 22px, 15px)
      .day
        &:before
          margin-left 3px
          bg-img('day.png', 18px, 18px)
        &:after
          content '白天'
      .night
        &:before
          margin-left 4px
          bg-img('night.png', 16px, 16px)
        &:after
          content '夜间'
      .download
        &:before
          margin-left 1px
          bg-img('download.png', 22px, 16px)
        &:after
          content '下载'
</style>
