<template>
  <div id="app">
    <reader-header :bar-show="barShow" :book="book"></reader-header>
    <reader-body :style="bgStyleObj" :chapter-id="chapterId" @click="toggleBar"></reader-body>
    <reader-footer :bar-show="barShow" :bg-style-objs.once="bgStyleObjs" :night-mode.sync="nightMode"></reader-footer>
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
        return this.nightMode ? nightModeStyle : this.bgStyleObjs[0]
      }
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
        nightMode: true,
        chapterId: '0',
        book: {},
        bgStyleObjs: [
          {background: '#f7eee5'},
          {background: '#e9dfc7'},
          {background: '#a4a4a4'},
          {background: '#cdefce'},
          {background: '#283548', color: '#7685a2'},
          {background: '#0f1410', color: '#4e534f'}
        ]
      }
    },
    created () {
      if (window.location.search.match(/chapter_id=(\d+)/)) {
        this.chapterId = RegExp.$1
      }
      this.$http.get('/api/detail').then(data => {
        if (data.body.result === OK) {
          this.book = data.body.item
        }
      })
      this.nightMode = storageGetter('night-mode') === 'true'
    }
  }
</script>

<style>
</style>
