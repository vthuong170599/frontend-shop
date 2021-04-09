<template>
  <div>
    <list-order :listOrder="listOrder" :fields="fields" />
    <b-pagination
      v-if="pages.last_page > 1"
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
import ListOrder from "~/components/Order/ListOrder.vue";
const FIELDS = [
  { key: "id", label: "id" },
  { key: "name", label: "Name" },
  { key: "date", label: "Date" },
  { key: "address", label: "Address" },
  { key: "phone", label: "phone" },
  { key: "name", label: "Name" },
  { key: "total_price", label: "Total price" },
  { key: "status", label: "status" },
  { key: "method", label: "method" },
  {
    key: "show_details",
    label: "",
    _style: "width:1%",
    sorter: false,
    filter: false,
  },
];
export default {
  components: { ListOrder },
  data() {
    return {
      listOrder: [],
      fields: FIELDS,
      pages: {},
      currentPage: 1,
    };
  },
  methods: {
    /**
     * call API list Order
     * @param Integer page
     */
    fetch(e, page) {
      axios
        .get("http://127.0.0.1:8000/api/order?page=" + page, {
          headers: {
            Authorization: `${$nuxt.$auth.getToken("local")}`,
          },
        })
        .then((res) => {
          this.listOrder = res.data.data;
          this.pages = res.data.meta;
        });
    },

  },
  mounted() {
    this.fetch();
  },
};
</script>