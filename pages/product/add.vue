<template>
  <create-edit-product @createProduct="createProduct" />
</template>
<script>
import CreateEditProduct from "~/components/Product/CreateEditProduct.vue";
import axios from "axios";
export default {
  components: {
    CreateEditProduct,
  },
  methods: {
    /**
     * create product
     * @param Object listProduct
     */
    async createProduct(listProduct) {
      // console.warn(' listProduct.thumb.target.files[0]',  listProduct.thumb.target.files[0]);
      const data = new FormData();
      data.append("cate_id", listProduct.cate_id);
      data.append("name", listProduct.name);
      data.append("desc", listProduct.desc);
      data.append("price", listProduct.price);
      data.append("discount", listProduct.discount);
      data.append("qty", listProduct.qty);
      data.append("thumb", listProduct.thumb);
      console.log(data.get('thumb'))
      const res = await axios
        .post("http://127.0.0.1:8000/api/product", data, {
          headers: {
            Authorization: `${$nuxt.$auth.getToken("local")}`,
            "Content-type": "multipart/form-data",
          },
        })
      if(res) {
          this.$router.push('/product')
      }
    },
  },
};
</script>