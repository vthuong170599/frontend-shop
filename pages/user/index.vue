<template>
  <profile-user :user="user"/>
</template>
<script>
import ProfileUser from "../../components/User/ProfileUser";
import axios from "axios";
import { URL } from "../../constant/constant";
export default {
  components: { ProfileUser },
  data() {
    return {
      user: {},
    };
  },
  methods: {
    fetch() {
      axios
        .get(URL + "user", {
          headers: {
            Authorization: `${$nuxt.$auth.getToken("local")}`,
          },
        })
        .then((res) => {
          this.user = res.data.user;
        //   console.log(this.user);
        });
    },
  },
  mounted() {
    this.fetch();
  },
};
</script>