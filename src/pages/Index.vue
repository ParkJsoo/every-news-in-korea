<template>
  <div>
    <q-list bordered>
      <q-item clickable v-ripple v-for="news in newsList" :key="news.link">
        <q-item-section class="text-h6">{{ news.title }}</q-item-section>
        <q-item-section side class="text-caption">{{ fromNow(news.pubDate) }}</q-item-section>
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
//날짜 형식을 위한 모듈
const moment = require("moment")
//언어를 한국어로 변경
moment.locale("ko-kr")

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
  methods: {
    //날짜 포맷을 지금으로부터 얼마전인지로 바꾸는 메소드
    fromNow(timestamp) {
      return moment(timestamp).fromNow()
    }
  }
}
</script>

<style>

</style>