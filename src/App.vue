<template>
  <div id="app">
    <reader-header :bar-show="barShow"></reader-header>
    <reader-body :chapter-id="chapterId" @click="toggleBar"></reader-body>
    <reader-footer :bar-show="barShow"></reader-footer>
  </div>
</template>

<script>
  import ReaderHeader from 'components/reader-header/reader-header'
  import ReaderBody from 'components/reader-body/reader-body'
  import ReaderFooter from 'components/reader-footer/reader-footer'
  import { getApiData } from 'common/js/utils.js'

  export default {
    components: {
      ReaderHeader,
      ReaderBody,
      ReaderFooter
    },
    methods: {
      toggleBar () {
        this.barShow = !this.barShow
        if (!this.barShow) {
          this.$broadcast('hide-bar')
        }
      }
    },
    events: {
      'reader-body-scroll' () {
        this.$broadcast('hide-bar')
        this.barShow = false
      }
    },
    data () {
      return {
        barShow: false,
        chapterId: '0',
        chapter: {},
        bookInfo: {},
        bookDetail: {}
      }
    },
    created () {
      if (window.location.search.match(/chapter_id=(\d+)/)) {
        this.chapterId = RegExp.$1
      }

      getApiData('/api/info', info => {
        this.bookInfo = info
      })

      getApiData('/api/detail', detail => {
        this.bookDetail = detail
      })
    }
  }
</script>

<style>
</style>
