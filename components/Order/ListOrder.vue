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
        :items="listOrder"
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
export default {
    freeSet,
  props: {
    listOrder: {
      type: Array,
      default: () => [],
    },
    fields: {
      type: Array,
      default: () => [],
    },
  },
};
</script>