<template>
  <CCard>
    <CCardHeader>
      <h3>Add Category</h3>
    </CCardHeader>
    <CCardBody>
      <CRow>
        <CCol sm="6">
          <CInput
            label="Name"
            placeholder="Enter your name"
            v-model="formProduct.name"
          />
        </CCol>
        <CCol sm="6">
          <CInput
            label="price"
            placeholder="Enter your price"
            v-model="formProduct.price"
          />
        </CCol>
      </CRow>
      <CRow>
        <CCol sm="6">
          <CInput
            label="discount"
            placeholder="Enter your discount"
            v-model="formProduct.discount"
          />
        </CCol>
        <CCol sm="6">
          <CInput
            label="quantity"
            placeholder="Enter your quantity"
            v-model="formProduct.qty"
          />
        </CCol>
      </CRow>
      <CRow>
        <CCol sm="6">
          <div class="form-group">
            <label for=""></label>
            <select class="custom-select" v-model="formProduct.cate_id">
              <option
                v-for="(category, index) in listCategory"
                :key="index"
                :value="category.id"
              >
                {{ category.name }}
              </option>
            </select>
          </div>
        </CCol>
        <CCol sm="6">
          <CInputFile type="flie" @change="getFile" abel="thumb" />
        </CCol>
      </CRow>
      <CRow>
        <CCol sm="12">
          <CTextarea
            label="Description"
            placeholder="Enter your description"
            v-model="formProduct.desc"
          >{{formProduct.desc}}
          </CTextarea>
        </CCol>
      </CRow>
    </CCardBody>
    <CCardFooter>
      <CButton
        v-if="!this.$route.params.id"
        color="primary"
        @click="createProduct()"
      >
        Submit
      </CButton>
      <CButton
        v-else
        color="primary"
        class="btn-click"
        @click="updateProduct(formProduct.id)"
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
      formProduct: {
        id: "",
        name: "",
        thumb: "",
        price: "",
        discount: "",
        cate_id: "",
        desc: "",
        qty: "",
      },
      listCategory: [],
    };
  },
  methods: {
    /**
     * create product
     * send data to page add
     */
    createProduct() {
      this.$emit("createProduct", this.formProduct);
    },

    getFile(files) {
      this.formProduct.thumb = files
    },

    /**
     * get category 
     */
    getCategory() {
      axios
        .get(URL+"category", {
          headers: {
            Authorization: `${$nuxt.$auth.getToken("local")}`,
          },
        })
        .then((res) => {
          this.listCategory = res.data.data;
        });
    },

    /**
     * get product by id
     * @param Integer id
     */
    getDataProduct(id) {
      axios
        .get(URL+"product/" + id, {
          headers: {
            Authorization: `${$nuxt.$auth.getToken("local")}`,
          },
        })
        .then((res) => {
          this.formProduct = res.data.data;
        });
    },

    /**
     * update product
     * send data to page _id
     * @param Integer id
     */
    updateProduct(id) {
      this.$emit("updateProduct", { id: id, data: this.formProduct });
    },
  },
  mounted() {
    this.getCategory();
    this.getDataProduct(this.$route.params.id);
  },
};
</script>