<template>
  <div class="card" v-if="this.product !== null" v-on:click="onViewProduct">
    <img :src="getImgProductPath" alt="Product Image" style="width: 100%" />
    <h1>{{ product.title }}</h1>
    <p class="price">{{ product.originPrice }}</p>
    <p>Còn {{ totalStock }} sản phẩm</p>
    <p><button>Add to Cart</button></p>
  </div>
</template>

<script>
export default {
  created() {},
  name: "product-card",
  props: {
    product: {
      type: Object,
      default: null,
    },
  },
  methods: {
    onViewProduct() {
      this.$emit("getSelectedProductId", this.product.id);
    },
  },
  computed: {
    hasData() {
      return this.product === null ? false : true;
    },
    getImgProductPath() {
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
  max-width: 300px;
  margin: auto;
  text-align: center;
  font-family: arial;
  box-shadow: 0px 14px 32px 0px rgba(0, 0, 0, 0.15);
  padding: 20px 10px 20px 10px;
}

@media only screen and (max-width: 600px) {
  .card {
    max-width: 40vw;
  }
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
  background-color: #a7c7e7;
  text-align: center;
  cursor: pointer;
  width: 100%;
  font-size: 18px;
}

.card button:hover {
  opacity: 0.7;
}
</style>