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
        <template #status="{ item }">
          <td>{{ getStatus(item.status) }}</td>
        </template>
        <template #method="{ item }">
          <div>
            <CButton color="success">
              <nuxt-link :to="`/order/${item.id}`">
                <CIcon :content="$options.freeSet.cilPencil" />
              </nuxt-link>
            </CButton>
          </div>
        </template>
        <template #show_details="{ item, index }">
          <td class="py-2">
            <CButton
              color="primary"
              variant="outline"
              square
              size="sm"
              @click="toggleDetails(item, index)"
            >
              {{ Boolean(item._toggled) ? "Hide" : "Detail" }}
            </CButton>
          </td>
        </template>

        <template #details="{ item }">
          <CCollapse
            :show="Boolean(item._toggled)"
            :duration="collapseDuration"
          >
            <CCard>
              <CCardHeader> <h6>Order Detail</h6> </CCardHeader>
              <CCardBody color="dark" class="text-white">
                <CCard body-wrapper>
                  <CDataTable :items="listOrderDetail" :fields="fieldsOrderDetail">
                    <template #product="{ item }">
                      <td>{{ getProduct(item.product) }}</td>
                    </template>
                    <template #price="{ item }">
                      <td>{{ getPrice(item.product) }}</td>
                    </template>
                  </CDataTable>
                </CCard>
              </CCardBody>
            </CCard>
          </CCollapse>
        </template>
      </CDataTable>
    </CCardBody>
  </CCard>
</template>
<script>
import { freeSet } from "@coreui/icons";
import { STATUS } from "../../constant/constant";
import listOrderDetail from "../OrderDetail/listOrderDetail.vue";
import axios from "axios";
const FIELDS_ORDER_DETAIL = [
  { key: "product", methods: "Product" },
  { key: "price", methods: "Price" },
  { key: "qty", methods: "Quantity" },
  { key: "sub_total", methods: "Sub Total" },
];
export default {
  components: { listOrderDetail },
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
  data() {
    return {
      status: STATUS,
      collapseDuration: 0,
      // listOrder: this.listOrder.map((item, id) => {
      //   return { item, id };
      // }),
      listOrderDetail: [],
      fieldsOrderDetail:FIELDS_ORDER_DETAIL
    };
  },
  methods: {
    getStatus(idOrder) {
      return this.status.find((stt, index) => {
        if (index == idOrder) {
          return stt;
        }
      });
    },

    toggleDetails(item, index) {
      console.log(item.id);
      this.$set(this.listOrder[index], "_toggled", !item._toggled);
      this.collapseDuration = 300;
      this.$nextTick(() => {
        this.collapseDuration = 0;
        axios
          .get("http://127.0.0.1:8000/api/show_by_order/"+item.id, {
            headers: {
              Authorization: `${$nuxt.$auth.getToken("local")}`,
            },
          })
          .then((res) => {
            this.listOrderDetail = res.data;
            console.log(res.data);
          });
      });
    },

     getProduct(product) {
        if(product == null) {
            return false;
        }
        return product.name
    },

    getPrice(product) {
        if(product == null) {
            return false;
        }
        return product.price
    },

  },
};
</script>
<style scoped>
.btn-detail:hover {
  color: #fff;
}
</style>