<template>
  <create-edit-category @updateCategory="updateCategory" />
</template>
<script>
import CreateEditCategory from "~/components/Category/CreateEditCategory.vue";
import axios from "axios";
import {URL} from '../../constant/constant';
export default {
  components: {
    CreateEditCategory,
  },
  methods: {
    /**
     * update category
     * @param Object data
     */
    updateCategory(data) {
      axios
        .put(URL+"category/" + data.id, data.data, {
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