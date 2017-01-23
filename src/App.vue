<template>
  <div id="app">
    <reader-header></reader-header>
    <reader-body></reader-body>
    <reader-footer></reader-footer>
  </div>
</template>

<script>
  import ReaderHeader from 'components/reader-header/reader-header'
  import ReaderBody from 'components/reader-body/reader-body'
  import ReaderFooter from 'components/reader-footer/reader-footer'
  import { getChapterContent, getApiData } from 'common/js/utils.js'

  export default {
    components: {
      ReaderHeader,
      ReaderBody,
      ReaderFooter
    },
    created () {
      getChapterContent('/api/link?chapter_id=1', (data) => console.log(data))
      getApiData('/api/info', data => console.log(data))
      getApiData('/api/detail', data => console.log(data))
    }
  }
</script>

<style>
</style>
