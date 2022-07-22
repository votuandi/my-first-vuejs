<template>
  <div class="list-pd-container" v-if="productsData !== []">
    <product-card
      v-on:getSelectedProductId="getSelectedProductId"
      v-for="product in productsData"
      :key="product.id"
      :product="product"
    />
  </div>
</template>

<script>
import ProductCard from "./ProductCard.vue";
export default {
  name: "list-product",
  components: {
    ProductCard,
  },
  props: {
    productsData: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  data() {
    return {};
  },
  methods: {
    getSelectedProductId(selectedProductId) {
      this.$emit("getSelectedProductId", selectedProductId);
    },
  },
  watch: {},
  computed: {},
};
</script>

<style>
.list-pd-container {
  align-items: center;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  /* justify-content: space-between; */
  padding: 10px 10px 10px 10px;
}
</style>