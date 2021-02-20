<template>
  <div>
    <q-list bordered>
      <q-item clickable v-ripple v-for="news in newsList" :key="news.link">
        <q-item-section>{{ news.title }}</q-item-section>
        <q-item-section>{{ news.pubDate }}</q-item-section>
      </q-item>
    </q-list>
  </div>
</template>

<script>
//rss xml 데이터를 json으로 변경을 위해 사용
const Parser = require('rss-parser');
const parser = new Parser();
//cors policy 해결을 위해 사용(proxy 우회)
const CORS_PROXY = "https://secret-ocean-49799.herokuapp.com"

export default {
  data() {
    return {
      newsList: []
    }
  },
  async created() {
    const response = await parser.parseURL(`${CORS_PROXY}/https://news.google.com/rss?hl=ko&gl=KR&ceid=KR:ko`)
    this.newsList = response.items
  },
  
}
</script>

<style>

</style>