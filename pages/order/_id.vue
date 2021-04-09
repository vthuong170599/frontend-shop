<template>
  <edit-order @updateOrder="updateOrder" />
</template>
<script>
import EditOrder from "~/components/Order/EditOrder.vue";
import axios from "axios";
import {URL} from '../../constant/constant';
export default {
  components: { EditOrder },
  methods: {
    updateOrder(listOrder) {
      axios.put(URL+"order/" + listOrder.id, listOrder, {
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