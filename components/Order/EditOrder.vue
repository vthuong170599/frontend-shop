<template>
  <CCard>
    <CCardHeader>
      <h3>Edit Order</h3>
    </CCardHeader>
    <CCardBody>
      <CRow>
        <CCol md="6">
          <CInput label="Name" disabled v-model="formOrder.name" />
        </CCol>
        <CCol md="6">
          <CInput label="Address" disabled v-model="formOrder.address" />
        </CCol>
      </CRow>
      <CRow>
        <CCol md="6">
          <CInput
            label="Total Price"
            disabled
            v-model="formOrder.total_price"
          />
        </CCol>
        <CCol md="6">
          <div class="form-group">
            <label for="">Status</label>
            <select
              class="form-control"
              v-model="formOrder.status"
              :disabled="setSelected()"
            >
              <option
                v-for="(stt, index) in status"
                :key="index"
                :value="index"
              >
                {{ stt }}
              </option>
            </select>
          </div>
        </CCol>
      </CRow>
      <CButton color="primary" @click="updateOrder(formOrder.id)"
        >Update</CButton
      >
    </CCardBody>
  </CCard>
</template>
<script>
import { STATUS } from "../../constant/constant";
import axios from "axios";
import {URL} from '../../constant/constant';
export default {
  data() {
    return {
      formOrder: {
        id: "",
        name: "",
        address: "",
        status: "",
        total_price: "",
      },
      status: STATUS,
    };
  },
  methods: {
      /**
       * get order by id
       * @param {Integer} id
       */
    getOrder(id) {
      axios
        .get(URL+"order/" + id, {
          headers: {
            Authorization: `${$nuxt.$auth.getToken("local")}`,
          },
        })
        .then((res) => {
          this.formOrder = res.data.data;
        });
    },

    /**
     * Update order
     * @param {Integer} id
     */
    updateOrder(id) {
      this.$emit("updateOrder", this.formOrder);
    },

    /**
     * set order selected
     */
    setSelected() {
      if (this.formOrder.status === 2 || this.formOrder.status === 3) {
        return true;
      } else {
        return false;
      }
    },
  },
  mounted() {
    this.getOrder(this.$route.params.id);
  },
};
</script>