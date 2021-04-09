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
        @click="createCategory()"
      >
        Submit
      </CButton>
      <CButton
        v-else
        color="primary"
        class="btn-click"
        @click="updateCategory(formCategory.id)"
      >
        Update
      </CButton>
    </CCardFooter>
  </CCard>
</template>
<script>
import axios from "axios";
import {URL} from '../../constant/constant';
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
    /**
     * create category
     * send data to page add
     */
    createCategory() {
      this.$emit("sendDataCategory", this.formCategory);
    },

    /**
     * update category
     * send data to page _id
     * @param Integer id
     */
    updateCategory(id){
        this.$emit('sendDataById', {id:id,data:this.formCategory});
    },

    /**
     * get category by id
     * @param Integer id
     */
    getDataCategory(id) {
      axios
        .get(URL+"category/" + id, {
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