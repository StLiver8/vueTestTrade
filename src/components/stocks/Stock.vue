<template>
  <div class="col-sm-5 card border-success mb-3" style="display: inline-block; margin-right: 30px;">
    <div class="card-header">
      <h3>
        {{stock.name}}
        <small>(Price: {{stock.price}})</small>
      </h3>
    </div>
    <div class="card-body text-success">
      <div class="float-left">
        <input type="number" class="form-control mb-3"
               v-model="quantity"
               placeholder="Quantity"
               :class="{danger: insufficientFunds}">
      </div>
      <div class="float-right">
        <button class="btn btn-success"
                @click="buyStock"
                :disabled="insufficientFunds || quantity <= 0">Buy</button>
      </div>
    </div>
  </div>
</template>

<script>
    export default {
        props: ['stock'],
        data() {
          return {
            quantity: 0
          }
        },
        computed: {
          funds() {
            return this.$store.getters.funds;
          },
          insufficientFunds() {
            return this.quantity * this.stock.price > this.funds;
          }
        },
        methods: {
          buyStock() {
            const order = {
              stockId: this.stock.id,
              stockPrice: this.stock.price,
              quantity: this.quantity
            };
            this.$store.dispatch('buyStock', order);
            this.quantity = 0;
          }
        }
    }
</script>

<style scoped>
  .danger {
    border: 1px solid darkred;
  }
</style>
