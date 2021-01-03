<template>
  <div class="col-sm-12 col-md-6">
    <div class="panel panel-info">
      <div class="panel-heading">
        <h3 class="panel-title">
          {{ stock.name }}
          <small>
            (Price: {{ stock.price }} | Quantity: {{ stock.quantity }})
          </small>
        </h3>
      </div>

      <div class="panel-body">
        <div class="pull-left">
          <input
            type="number"
            class="form-control"
            placeholder="Quantity"
            v-model.number="quantity"
            :class="{ danger: insufficientQuantity }"
          />
        </div>
        <div class="pull-right">
          <button
            class="btn btn-success"
            @click="sellStock"
            :disabled="isDisabled || insufficientQuantity"
          >
            {{ insufficientQuantity ? "Not enough Stocks" : "Sell" }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.danger {
  border: 1px solid red;
}
</style>

<script>
import { mapActions } from "vuex";

export default {
  props: ["stock"],
  data() {
    return {
      quantity: 0,
    };
  },
  methods: {
    ...mapActions({
      placeSellOrder: "sellStock",
    }),
    sellStock() {
      const order = {
        stockId: this.stock.id,
        stockPrice: this.stock.price,
        quantity: this.quantity,
      };
      this.placeSellOrder(order);
      this.quantity = 0;
    },
  },
  computed: {
    isDisabled() {
      return this.quantity <= 0 || !Number.isInteger(this.quantity);
    },
    insufficientQuantity() {
      return this.quantity > this.stock.quantity;
    },
  },
};
</script>
