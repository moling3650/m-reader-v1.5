<template>
  <div id="app">
  </div>
</template>

<script>
  import { getChapterContent, getApiData } from 'common/js/utils.js'

  export default {
    created () {
      getChapterContent('/api/link?chapter_id=1', (data) => console.log(data))
      getApiData('/api/info', data => console.log(data))
      getApiData('/api/detail', data => console.log(data))
    }
  }
</script>

<style>
</style>
