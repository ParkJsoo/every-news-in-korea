<template>
  <div>
    <q-table
      flat
      :data="newsList"
      :columns="columns"
      :filter="filter"
      row-key="name"
      @row-click="rowClick"
      :pagination.sync="pagination"
    >
      <template v-slot:body-cell-title="props">
        <q-td :props="props">
          <div
            class="text-h6 text-grey bg-purple-7"
            style="white-space: normal"
          >
            {{ props.value }}
          </div>
          <div class="my-table-details">
            {{ props.row.details }}
          </div>
        </q-td>
      </template>
      <template v-slot:body-cell-pubDate="props">
        <q-td :props="props">
          <div>
            {{ formatDate(props.value) }}
          </div>
          <div class="my-table-details">
            {{ props.row.details }}
          </div>
        </q-td>
      </template>
      <template v-slot:top-right>
        <q-input
          borderless
          dense
          debounce="300"
          v-model="filter"
          placeholder="검색"
        >
          <template v-slot:append>
            <q-icon class="fab fa-searchengin" />
          </template>
        </q-input>
      </template>
    </q-table>
  </div>
</template>

<script>
//rss xml 데이터를 json으로 변경을 위해 사용
const Parser = require("rss-parser");
const parser = new Parser();
//cors policy 해결을 위해 사용(proxy 우회)
const CORS_PROXY = "https://secret-ocean-49799.herokuapp.com";
//날짜 형식을 위한 모듈
const moment = require("moment");
//언어를 한국어로 변경
moment.locale("ko-kr");
// Quasar Date Utils
import { date } from "quasar";

export default {
  data() {
    return {
      pagination: {
        rowsPerPage: 20 // current rows per page being displayed
      },
      newsList: [],
      filter: "",
      columns: [
        {
          name: "title",
          required: true,
          label: "기사 제목",
          align: "left",
          field: row => row.title,
          format: val => `${val}`,
          sortable: true
        },
        {
          name: "pubDate",
          required: true,
          label: "발행일",
          align: "center",
          field: row => row.pubDate,
          format: val => `${val}`,
          sortable: true
        }
      ]
    };
  },
  async created() {
    const response = await parser.parseURL(
      `${CORS_PROXY}/https://news.google.com/rss?hl=ko&gl=KR&ceid=KR:ko`
    );
    this.newsList = response.items;
    this.$q.notify({
      message: "뉴스를 성공적으로 불러왔습니다.",
      position: "center"
    });
  },
  methods: {
    //날짜 포맷을 지금으로부터 얼마전인지로 바꾸는 메소드
    // fromNow(timestamp) {
    //   return moment(timestamp).fromNow();
    // },
    // Quasar Date Utils를 사용하여 날짜 포맷 변경
    formatDate(timestamp) {
      return date.formatDate(timestamp, "YYYY-MM-DD HH:MM");
    },
    rowClick(evt, row, index) {
      window.open(this.newsList[index].link);
    }
  }
};
</script>

<style></style>
