<template>
  <CCard>
    <CCardHeader>
      <h3>list Category</h3>
    </CCardHeader>
    <CCardBody>
      <CButton color="primary" class="m-2 btn_add">
        <nuxt-link to="/product/add" class="text-white d-block w-100">
          + Add</nuxt-link
        >
      </CButton>
      <CDataTable
        :items="listProduct"
        :fields="fields"
        items-per-page-select
        :items-per-page="5"
        pagination
      >
        <template #thumb="{ item }">
          <div>
            <td class="img">
              <img
                :src="require(`@/assets/img/logo.png`)"
                alt=""
                class="img-fluid w-75"
              />
            </td>
          </div>
        </template>
        <template #method="{ item }">
          <td class="py-2">
            <CButton color="success">
              <nuxt-link :to="`/product/${item.id}`">
                <CIcon :content="$options.freeSet.cilPencil" />
              </nuxt-link>
            </CButton>
            <CButton color="danger" @click="deleteProduct(item.id)">
              <CIcon :content="$options.freeSet.cilTrash" />
            </CButton>
          </td>
        </template>
      </CDataTable>
    </CCardBody>
  </CCard>
</template>
<script>
import { freeSet } from "@coreui/icons";
import axios from "axios";
import swal from "sweetalert2";
export default {
  freeSet,
  props: {
    listProduct: {
      type: Array,
      default: () => [],
    },
    fields: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    deleteProduct(id) {
      swal
        .fire({
          title: "Are you sure?",
          text: "You won't be able to revert this!",
          icon: "warning",
          showCancelButton: true,
          confirmButtonColor: "#3085d6",
          cancelButtonColor: "#d33",
          confirmButtonText: "Yes, delete it!",
        })
        .then((result) => {
          console.log(result);
          if (result.value) {
            axios
              .delete("http://127.0.0.1:8000/api/product/" + id, {
                headers: {
                  Authorization: `${$nuxt.$auth.getToken("local")}`,
                },
              })
              .then((res) => {
                this.$emit("getListProduct", this.listProduct);
              });
            swal.fire("Deleted!", "Your file has been deleted.", "success");
          }
        });
    },
  },
};
</script>
<style scoped>
.img {
  width: 100px;
}
</style>