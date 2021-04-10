<template>
  <change-password @changePassword="changePassword" />
</template>
<script>
import ChangePassword from "../../components/User/ChangePassword";
import axios from "axios";
import { URL } from "../../constant/constant";
export default {
  components: { ChangePassword },
  methods: {
    changePassword(dataPassword) {
      axios
        .put(URL + "user/change-password/"+$nuxt.$auth.user.id, dataPassword,{
          headers: {
            Authorization: `${$nuxt.$auth.getToken("local")}`,
          },
        })
        .then((res) => {
         this.$router.push('/');
        });
    },
  },
};
</script>