<template>
  <edit-order @updateOrder="updateOrder" />
</template>
<script>
import EditOrder from "~/components/Order/EditOrder.vue";
import axios from "axios";
export default {
  components: { EditOrder },
  methods: {
    updateOrder(listOrder) {
      axios.put("http://127.0.0.1:8000/api/order/" + listOrder.id, listOrder, {
        headers: {
          Authorization: `${$nuxt.$auth.getToken("local")}`,
        },
      }).then(res=>{
          this.$router.push('/order');
      });
    },
  },
};
</script>