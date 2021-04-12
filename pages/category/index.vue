<template>
  <div>
       <search @sendKeyword="getKeyword" />
    <list-category :listCategory="listCategory" :fields="fields" @getListCategory="fetch"/>
    <b-pagination
      v-if="pages.last_page>1"
      v-model="currentPage"
      :total-rows="pages.total"
      :per-page="pages.per_page"
      @page-click="fetch"
      prev-text="Prev"
      next-text="Next"
    ></b-pagination>
  </div>
</template>
<script>
import axios from "axios";
import ListCategory from "~/components/Category/ListCategory.vue";
import {URL} from '../../constant/constant';
import search from "~/components/common/search";
const FIELDS = [
  { key: "id", label: "id" },
  { key: "name", label: "name" },
  { key: "desc", label: "desc" },
  { key: "method", label: "method" },
];
export default {
  components: { ListCategory,search },
  data() {
    return {
      listCategory: [],
      fields: FIELDS,
      pages: {},
      currentPage: 1,
    };
  },
  methods: {
    /**
     * Call API List category
     * @param Integer page
     */
    fetch(e, page) {
      axios
        .get(URL+"category?page=" + page, {
          headers: {
            Authorization: `${$nuxt.$auth.getToken("local")}`,
          },
        })
        .then((res) => {
          this.listCategory = res.data.data;
          this.pages = res.data.meta;
        });
    },

     getKeyword(value) {
      this.search = value;
      axios
        .get(URL + "categories?name=" + this.search, {
          headers: {
            Authorization: `${$nuxt.$auth.getToken("local")}`,
          },
        })
        .then((res) => {
          this.listCategory = res.data;
        });
    },
  },
  mounted() {
    this.fetch();
  },
};
</script>