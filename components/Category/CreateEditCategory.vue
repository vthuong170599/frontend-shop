<template>
  <CCard>
    <CCardHeader>
      <h3>Add Category</h3>
    </CCardHeader>
    <CCardBody>
      <CRow>
        <CCol sm="12">
          <CInput
            label="Name"
            placeholder="Enter your name"
            v-model="formCategory.name"
          />
        </CCol>
      </CRow>
      <CRow>
        <CCol sm="12">
          <CTextarea
            label="Description"
            placeholder="Enter your description"
            v-model="formCategory.desc"
          >
          </CTextarea>
        </CCol>
      </CRow>
    </CCardBody>
    <CCardFooter>
      <CButton
        v-if="!this.$route.params.id"
        color="primary"
        @click="sendDataCategory()"
      >
        Submit
      </CButton>
      <CButton
        v-else
        color="primary"
        class="btn-click"
        @click="sendDataById(formCategory.id)"
      >
        Update
      </CButton>
    </CCardFooter>
  </CCard>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      formCategory: {
        name: "",
        desc: "",
      },
    };
  },
  methods: {
    sendDataCategory() {
      this.$emit("sendDataCategory", this.formCategory);
    },
    sendDataById(id){
        this.$emit('sendDataById', {id:id,data:this.formCategory});
    },
    getDataCategory(id) {
      axios
        .get("http://127.0.0.1:8000/api/category/" + id, {
          headers: {
            Authorization: `${$nuxt.$auth.getToken("local")}`,
          },
        })
        .then((res) => {
          this.formCategory = res.data.data;
        });
    },
  },
  mounted() {
    this.getDataCategory(this.$route.params.id);
  },
};
</script>