<template>
  <div class="card" v-if="this.product !== null">
    <img :src="getmgProductPath" alt="Product Image" style="width: 100%" />
    <h1>{{ product.tittle }}</h1>
    <p class="price">{{ product.originPrice }}</p>
    <p>Còn {{ totalStock }} sản phẩm</p>
    <p><button>Add to Cart</button></p>
  </div>
</template>

<script>
export default {
  created() {},
  name: "test-tag",
  props: {
    product: {
      type: Object,
      default: null,
    },
  },
  computed: {
    hasData() {
      return this.product === null ? false : true;
    },
    getmgProductPath() {
      return require(`@/assets/img/${
        this.product.subProducts[this.product.defaultSubProduct].id
      }.jpg`);
    },
    totalStock() {
      let total = 0;
      this.product.subProducts.forEach((sp) => {
        total += sp.stock;
      });
      return total;
    },
  },
};
</script>

<style>
.card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  max-width: 300px;
  margin: auto;
  text-align: center;
  font-family: arial;
}

.price {
  color: grey;
  font-size: 22px;
}

.card button {
  border: none;
  outline: 0;
  padding: 12px;
  color: white;
  background-color: #000;
  text-align: center;
  cursor: pointer;
  width: 100%;
  font-size: 18px;
}

.card button:hover {
  opacity: 0.7;
}
</style>