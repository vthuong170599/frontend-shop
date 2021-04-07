<template>
  <CCard>
    <CCardHeader>
      <h3>list Category</h3>
    </CCardHeader>
    <CCardBody>
      <CButton color="primary" class="m-2 btn_add">
        <nuxt-link to="/category/add" class="text-white d-block w-100">
          + Add</nuxt-link
        >
      </CButton>
      <CDataTable
        :items="listCategory"
        :fields="fields"
        items-per-page-select
        :items-per-page="5"
        pagination
      >
        <template #method="{ item }">
          <div>
            <CButton color="success">
              <nuxt-link :to="`/category/${item.id}`">
                <CIcon :content="$options.freeSet.cilPencil" />
              </nuxt-link>
            </CButton>
            <CButton color="danger" @click="deleteCategory(item.id)">
              <CIcon :content="$options.freeSet.cilTrash" />
            </CButton>
          </div>
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
    listCategory: {
      type: Array,
      default: () => [],
    },
    fields: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    deleteCategory(id) {
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
              .delete("http://127.0.0.1:8000/api/category/" + id, {
                headers: {
                  Authorization: `${$nuxt.$auth.getToken("local")}`,
                },
              })
              .then((res) => {
                this.$emit("getListCategory", this.listCategory);
              });
            swal.fire("Deleted!", "Your file has been deleted.", "success");
          }
        });
    },
  },
};
</script>