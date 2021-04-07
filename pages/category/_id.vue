<template>
  <create-edit-category @sendDataById="getDataCategory" />
</template>
<script>
import CreateEditCategory from "~/components/Category/CreateEditCategory.vue";
import axios from "axios";
export default {
  components: {
    CreateEditCategory,
  },
  methods: {
    getDataCategory(data) {
      axios
        .put("http://127.0.0.1:8000/api/category/" + data.id, data.data, {
          headers: {
            Authorization: `${$nuxt.$auth.getToken("local")}`,
          },
        })
        .then((res) => {
          this.$router.push("/category");
        });
    },
  },
};
</script>