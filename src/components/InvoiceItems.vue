<template>
  <div class="main-content">
    <b-table class="inv-table" :items="invoiceItems" :fields="fields">
    </b-table>
    <div class="d-flex summary">
      <div class="d-flex flex-column">
        <span class="">Sub Total</span> <span class="">Tax</span>
        <span class="pt-5 font-weight-bold">Total Amount(EUR)</span>
      </div>
      <div class="d-flex flex-column">
        <span> €{{ subTotal }}</span
        ><span>{{ totalTaxes }}</span
        ><span class="pt-5"> {{ total }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";

export default {
  name: "items",
  // props: ["invoiceItems"],
  data() {
    return {
      fields: ["Description", "price", "quantity", "SubAmount"]
    };
  },
  mounted() {
    // this.invoiceItems.forEach(item => {
    //   item.SubAmount = (parseInt(item.price) * parseInt(item.quantity)).toFixed(
    //     2
    //   );
    //   if (!item._tax) item._tax = 1;
    // });
    // console.log(this.invoiceItems);
  },
  computed: {
    ...mapGetters({
      invoiceItems: "getInvoiceItems"
    }),
    subTotal() {
      // let subt = 0;
      // this.invoiceItems.forEach(item => {
      //   console.log(item["SubAmount"]);
      //   subt += parseFloat(item.SubAmount);
      // });
      // return subt;
      console.log(this.invoiceItems);
      
      if (this.invoiceItems) {
        return this.invoiceItems
          .reduce((acc, current) => acc + parseFloat(current.SubAmount), 0)
          .toFixed(2);
      } else {
        return 0;
      }
    },
    total() {
      if (this.invoiceItems) {
        return (parseFloat(this.subTotal) + parseFloat(this.totalTaxes)).toFixed(2);
      } else {
        return 0;
      }
    },
    totalTaxes() {
      if (this.invoiceItems) {
        return this.invoiceItems
          .reduce((acc, cur) => acc + parseFloat(cur._tax), 0)
          .toFixed(2);
      } else {
        return 0;
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.main-content {
  width: 100%;
}

.summary {
  display: flex;
  text-align: right;
  justify-content: flex-end;
  padding-right: 4.5rem;

  & div {
    padding-bottom: 2rem;
    text-align: right;

    &:first-of-type {
      padding-right: 5rem;
    }
  }
}
</style>
