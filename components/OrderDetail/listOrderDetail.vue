  <template>
  <div>
     <CCard body-wrapper>
        <CDataTable
          :items="listOrderDetail"
          :fields="fields"
        >
          <template #product="{ item }">
            <td>{{ getProduct(item.product) }}</td>
          </template>
          <template #price="{ item }">
            <td>{{ getPrice(item.product) }}</td>
          </template>
        </CDataTable>
      </CCard>
  </div>
</template>
<script>
import axios from "axios";
const FIELDS = [
  { key: "product", methods: "Product" },
  { key: "price", methods: "Price" },
  { key: "qty", methods: "Quantity" },
  { key: "sub_total", methods: "Sub Total" },
];
export default {
  data() {
    return {
      collapse: false,
      listOrderDetail: [],
      fields: FIELDS,
    };
  },
  methods: {
    getOrderDetail(id) {
      axios
        .get("http://127.0.0.1:8000/api/order_detail", {
          headers: {
            Authorization: `${$nuxt.$auth.getToken("local")}`,
          },
        })
        .then((res) => {
          this.listOrderDetail = res.data;
        });
    },

    getProduct(product) {
        if(product == null) {
            return false;
        }
        return product.name
    }
    ,
    getPrice(product) {
        if(product == null) {
            return false;
        }
        return product.price
    },

    getIdOrder(id){

    }
  },
  mounted() {
    this.getOrderDetail();
  },
};
</script>