<template>
  <div id="app">
    <reader-header :bar-show="barShow" :book="book"></reader-header>
    <reader-body :style="bgStyleObj" :chapter-id="chapterId" :font-size="fontSize" @click="toggleBar"></reader-body>
    <reader-footer :bar-show="barShow" :bg-style-objs.once="bgStyleObjs" :night-mode.sync="nightMode" :bg-type.sync="bgType" :font-size.sync="fontSize" :chapter-id.sync="chapterId" :latest-id="book.latest_id"></reader-footer>
  </div>
</template>

<script>
  import ReaderHeader from 'components/reader-header/reader-header'
  import ReaderBody from 'components/reader-body/reader-body'
  import ReaderFooter from 'components/reader-footer/reader-footer'
  import { storageGetter } from 'common/js/utils.js'

  const OK = 0

  export default {
    components: {
      ReaderHeader,
      ReaderBody,
      ReaderFooter
    },
    computed: {
      bgStyleObj () {
        let nightModeStyle = this.bgStyleObjs[5]
        let dayModeStyle = this.bgStyleObjs[this.bgType]
        return this.nightMode ? nightModeStyle : dayModeStyle
      }
    },
    methods: {
      toggleBar () {
        this.barShow = !this.barShow
        if (!this.barShow) {
          this.$broadcast('hide_bar')
        }
      },
      hideBar () {
        this.$broadcast('hide_bar')
        this.barShow = false
      }
    },
    events: {
      'reader_body_scroll' () {
        this.hideBar()
      },
      'change_chapter' () {
        this.$broadcast('reader_body_refresh')
      }
    },
    data () {
      return {
        barShow: false,
        nightMode: false,
        chapterId: 0,
        book: {},
        bgStyleObjs: [
          {background: '#f7eee5'},
          {background: '#e9dfc7'},
          {background: '#a4a4a4'},
          {background: '#cdefce'},
          {background: '#283548', color: '#7685a2'},
          {background: '#0f1410', color: '#4e534f'}
        ],
        bgType: 0,
        fontSize: 14
      }
    },
    created () {
      if (window.location.search.match(/chapter_id=(\d+)/)) {
        this.chapterId = parseInt(RegExp.$1)
      }
      this.$http.get('/api/detail').then(data => {
        if (data.body.result === OK) {
          this.book = data.body.item
        }
      })
      this.nightMode = storageGetter('night-mode') === 'true'
      this.bgType = parseInt(storageGetter('background-type')) || 0
      this.fontSize = parseInt(storageGetter('font-size')) || 14
    }
  }
</script>

<style>
</style>
