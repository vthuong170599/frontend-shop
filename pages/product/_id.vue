<template>
  <create-edit-product @updateProduct="updateProduct" />
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
     * update Product
     * @param Object ListProduct
     */
    updateProduct(dataProduct) {
      console.log(dataProduct.data);
      // console.warn(' listProduct.thumb.target.files[0]',  dataProduct.data.thumb.target);
      let data = new FormData();
      data.append("cate_id", dataProduct.data.cate_id);
      data.append("name", dataProduct.data.name);
      data.append("desc", dataProduct.data.desc);
      data.append("price", dataProduct.data.price);
      data.append("discount", dataProduct.data.discount);
      data.append("qty", dataProduct.data.qty);
      data.append("thumb", dataProduct.data.thumb);
      console.log(data.get('name'));
      axios
        .post("http://127.0.0.1:8000/api/product/" + dataProduct.id, data, {
          headers: {
            Authorization: `${$nuxt.$auth.getToken("local")}`,
            "Content-type": "multipart/form-data",
          },
        })
        .then((res) => {
          this.$router.push("/product");
        });
    },
  },
};
</script>