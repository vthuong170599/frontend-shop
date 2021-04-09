<template>
  <div>
    <create-edit-category @createCategory="createCategory" />
  </div>
</template>
<script>
import CreateEditCategory from "~/components/Category/CreateEditCategory.vue";
import axios from "axios";
import {URL} from '../../constant/constant';
export default {
  components: { CreateEditCategory },
  methods: {
    /**
     * create category
     * @param Object data
     */
    createCategory(data) {
      axios
        .post(
          URL+"category",
          data,
          {
            headers: {
              Authorization: `${$nuxt.$auth.getToken("local")}`,
            },
          }
        )
        .then((res) => {
          this.$router.push("/category");
        });
    },
  },
};
</script>