<template>
  <b-container>
    <h1>Top 10 Heroes</h1>
    <b-row>
      <b-col sm="3">
        <b-input placeholder="Search" v-model="name"></b-input>
      </b-col>
    </b-row>
    <br />
    <b-table bordered striped hover :items="list" :fields="fields" :filter="name">
      <template #cell(img)="row">
        <div>
          <b-img :src="'https://api.opendota.com' + row.item.img" alt="Center image"></b-img>
        </div>
      </template>
    </b-table>
  </b-container>
</template>
<script>
import Vue from "vue";
import VueAxios from "vue-axios";
import axios from "axios";
Vue.use(VueAxios, axios);
export default {
  name: "Heroes",
  data() {
    return {
      fields: [
        {
          key: "img",
          sortable: true,
          label: "Hero Image",
        },
        {
          key: "localized_name",
          sortable: true,
          label: "Hero Name",
        },
        {
          key: "pro_win",
          sortable: true,
          label: "PRO Win Rate",
        },
        {
          key: "pro_pick",
          sortable: true,
          label: "PRO Pick Count",
        },
        {
          key: "pro_ban",
          sortable: true,
          label: "PRO Ban Count",
        },
      ],
      list: undefined,
      name: ''
    };
  },
  mounted() {
    Vue.axios.get("https://api.opendota.com/api/heroStats").then((res) => {
      this.list = res.data
        .sort((a, b) => (a.pro_win < b.pro_win ? 1 : -1))
        .slice([0], [10]);
    });
  },
};
</script>