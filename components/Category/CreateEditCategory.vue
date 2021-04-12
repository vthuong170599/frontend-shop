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
          <p v-if="Object.keys(error).length > 0" class="text text-danger">
            {{ error.name }}
          </p>
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
          <p v-if="Object.keys(error).length > 0" class="text text-danger">
            {{ error.desc }}
          </p>
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
import { URL } from "../../constant/constant";
export default {
  data() {
    return {
      formCategory: {
        name: "",
        desc: "",
      },
      error: {},
    };
  },
  methods: {
    /**
     * create category
     * send data to page add
     */
    createCategory() {
      this.validate();
      if (Object.keys(this.error).length > 0) {
        return this.error;
      } else {
        this.$emit("createCategory", this.formCategory);
      }
    },

    /**
     * update category
     * send data to page _id
     * @param Integer id
     */
    updateCategory(id) {
      this.validate();
      if (Object.keys(this.error).length > 0) {
        return this.error;
      } else {
        this.$emit("updateCategory", { id: id, data: this.formCategory });
      }
    },

    /**
     * get category by id
     * @param Integer id
     */
    getDataCategory(id) {
      axios
        .get(URL + "category/" + id, {
          headers: {
            Authorization: `${$nuxt.$auth.getToken("local")}`,
          },
        })
        .then((res) => {
          this.formCategory = res.data.data;
        });
    },

    validate() {
      this.error = {};
      if (this.formCategory.name == "") {
        this.error.name = "name khong duoc de trong";
      } else if (this.formCategory.desc == "") {
        this.error.desc = "desc khong duoc de trong";
      }
    },
  },
  mounted() {
    this.getDataCategory(this.$route.params.id);
  },
};
</script>