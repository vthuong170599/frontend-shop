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
          <p v-if="Object.keys(error).length > 0" class="text text-danger">
            {{ error.name }}
          </p>
        </CCol>
        <CCol sm="6">
          <CInput
            label="price"
            placeholder="Enter your price"
            v-model="formProduct.price"
          />
          <p v-if="Object.keys(error).length > 0" class="text text-danger">
            {{ error.price }}
          </p>
        </CCol>
      </CRow>
      <CRow>
        <CCol sm="6">
          <CInput
            label="discount"
            placeholder="Enter your discount"
            v-model="formProduct.discount"
          />
          <p v-if="Object.keys(error).length > 0" class="text text-danger">
            {{ error.disount }}
          </p>
        </CCol>
        <CCol sm="6">
          <CInput
            label="quantity"
            placeholder="Enter your quantity"
            v-model="formProduct.qty"
          />
          <p v-if="Object.keys(error).length > 0" class="text text-danger">
            {{ error.qty }}
          </p>
        </CCol>
      </CRow>
      <CRow>
        <CCol sm="6">
          <div class="form-group">
            <label for="">category</label>
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
          <p v-if="Object.keys(error).length > 0" class="text text-danger">
            {{ error.cate_id }}
          </p>
        </CCol>
        <CCol sm="6">
          <CInputFile
            label="thumb"
            type="flie"
            @change="getFile"
            abel="thumb"
          />
          <p v-if="Object.keys(error).length > 0" class="text text-danger">
            {{ error.thumb }}
          </p>
        </CCol>
      </CRow>
      <CRow>
        <CCol sm="12">
          <CTextarea
            label="Description"
            placeholder="Enter your description"
            v-model="formProduct.desc"
            >{{ formProduct.desc }}
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
import { URL } from "../../constant/constant";
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
      error: {},
    };
  },
  methods: {
    /**
     * create product
     * send data to page add
     */
    createProduct() {
      this.validate();
      if (Object.keys(this.error).length > 0) {
        return this.error;
      }
      this.$emit("createProduct", this.formProduct);
    },

    getFile(files) {
      this.formProduct.thumb = files;
    },

    /**
     * get category
     */
    getCategory() {
      axios
        .get(URL + "category", {
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
        .get(URL + "product/" + id, {
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
      this.validate();
      if (Object.keys(this.error).length > 0) {
        return this.error;
      }
      this.$emit("updateProduct", { id: id, data: this.formProduct });
    },

    validate() {
      this.error = {};
      if (this.formProduct.name == "") {
        this.error.name = "name khong duoc de trong";
      } else if (this.formProduct.price == "") {
        this.error.price = "price khong duoc de trong";
      } else if (this.formProduct.discount == "") {
        this.error.discount = "discount khong duoc de trong";
      } else if (this.formProduct.qty == "") {
        this.error.qty = "quantity khong duoc de trong";
      } else if (this.formProduct.cate_id == "") {
        this.error.cate_id = "category khong duoc de trong";
      } else if (this.formProduct.thumb == "") {
        this.error.thumb = "thumb khong duoc de trong";
      } else if (this.formProduct.desc == "") {
        this.error.desc = "desc khong duoc de trong";
      }
    },
  },
  mounted() {
    this.getCategory();
    this.getDataProduct(this.$route.params.id);
  },
};
</script>