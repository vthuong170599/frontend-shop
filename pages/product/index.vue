<template>
  <div>
    <list-product
      :listProduct="listProduct"
      :fields="fields"
      @getListProduct="fetch"
    />
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
import ListProduct from "~/components/Product/ListProduct.vue";
import axios from "axios";
const FIELDS = [
  { key: "id", label: "id" },
  { key: "name", label: "Name" },
  { key: "thumb", label: "Thumb" },
  { key: "price", label: "price" },
  { key: "sale_price", label: "Sale price" },
  { key: "discount", label: "Discount" },
  { key: "category", label: "Category" },
  { key: "qty", label: "Quantity" },
  { key: "method", label: "method" },
];
export default {
  components: { ListProduct },
  data() {
    return {
      listProduct: [],
      fields: FIELDS,
      pages: {},
      currentPage: 1,
    };
  },
  methods: {
    fetch(e, page) {
      axios
        .get("http://127.0.0.1:8000/api/product?page=" + page, {
          headers: {
            Authorization: `${$nuxt.$auth.getToken("local")}`,
          },
        })
        .then((res) => {
          this.listProduct = res.data.data;
          this.pages = res.data.meta;
        });
    },
  },
  mounted(){
      this.fetch();
  }
};
</script>