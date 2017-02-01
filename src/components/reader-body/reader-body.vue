<template>
  <div class="reader-body" @scroll="bodyScroll" v-el:reader-body>
    <ul class="chapters" :style="{fontSize: fontSize + 'px'}">
      <li class="chapter" v-for="chapter in chapters" track-by="$index">
        <h1 class="title">{{ chapter.t }}</h1>
        <p class="paragraph" v-for="paragraph in chapter.p" track-by="$index">{{ paragraph }}</p>
      </li>
    </ul>
  </div>
</template>

<script type="text/ecmascript-6">
  import { getChapterContent } from 'common/js/utils.js'

  export default {
    props: {
      chapterId: Number,
      fontSize: Number
    },
    methods: {
      bodyScroll () {
        if (this.timer === null) {
          this.timer = setTimeout(() => {
            clearTimeout(this.timer)
            this.timer = null
            this.$dispatch('reader_body_scroll')
            let rb = this.$els.readerBody
            let isBottom = (rb.scrollHeight - rb.scrollTop - rb.clientHeight) < (rb.clientHeight * 0.5)

            if (isBottom) {
              getChapterContent(`/api/link?chapter_id=${++this.chapterId}`, chapter => {
                this.chapters.push(chapter)
              })
            }
          }, 500)
        }
      }
    },
    events: {
      'reader_body_refresh' () {
        this.$nextTick(() => {
          getChapterContent(`/api/link?chapter_id=${this.chapterId}`, chapter => {
            this.chapters.splice(0, this.chapters.length, chapter)
            this.$els.readerBody.scrollTop = 0
          })
        })
      }
    },
    data () {
      return {
        chapters: [],
        timer: null
      }
    },
    ready () {
      getChapterContent(`/api/link?chapter_id=${this.chapterId}`, chapter => {
        this.chapters.splice(0, this.chapters.length, chapter)
      })
    }
  }
</script>

<style lang="stylus" scoped>
  .reader-body
    position absolute
    top 0
    right 0
    bottom 0
    left 0
    z-index 1
    max-width 900px
    padding 15px 0
    margin 0 auto
    overflow auto
    color #333
  .chapters
    margin 0 15px
  .chapter
    padding-bottom 50px
    .title
      border-bottom 1px solid
      margin-bottom 20px
      font-size 20px
      font-weight bold
      line-height 31px
      color #736357
      letter-spacing 2px
    .paragraph
      overflow hidden
      margin .5em 0
      line-height 24px
      letter-spacing 0
      text-align justify
      text-indent 2em
</style>
