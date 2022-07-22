<template>
  <div id="app">
    <home-header
      :amountInCart="getAmountInCart"
      v-on:searchBySearachBar="getSearchContentByHeader"
      v-on:showHomePage="showHomePage"
    />

    <div class="body">
      <list-product
        v-on:getSelectedProductId="getSelectedProductId"
        v-if="bodyContentActivated === 'list-product'"
        :productsData="listProducts"
      />

      <product-detail
        v-on:getAddedProduct="getAddedProduct"
        v-if="bodyContentActivated === 'product-detail'"
        :product="getProductById(selectedProductId)"
      />
    </div>

    <home-footer class="home-footer" />
  </div>
</template>

<script>
import HomeHeader from "./components/HomeHeader.vue";
import HomeFooter from "./components/HomeFooter.vue";
import ListProduct from "./components/ListProduct.vue";
import jsonListProducts from "./assets/data/Products.json";
import ProductDetail from "./components/ProductDetail.vue";
export default {
  name: "App",
  components: {
    HomeHeader,
    HomeFooter,
    ListProduct,
    ProductDetail,
  },
  data() {
    return {
      //body
      bodyContentActivated: "list-product",
      // bodyContentActivated: "product-detail",

      //data
      listProducts: [],
      selectedProductId: null,
      cart: {},
    };
  },
  methods: {
    getSearchContentByHeader(searchContent) {
      console.log(searchContent);
      this.listProducts = jsonListProducts;
    },

    getProductById(id) {
      let result = null;
      this.listProducts.forEach((product) => {
        if (product.id === id) {
          result = product;
        }
      });
      return result;
    },
    getSelectedProductId(selectedProductId) {
      let selectedProduct = this.getProductById(selectedProductId);
      if (selectedProduct === null) {
        alert("404 PRODUCT NOT FOUND");
      } else {
        this.selectedProductId = selectedProductId;
        this.bodyContentActivated = "product-detail";
      }
    },
    showHomePage() {
      this.bodyContentActivated = "list-product";
    },
    getAddedProduct(idProduct, idSubProduct, amount) {
      if (!Object.keys(this.cart).includes(idProduct)) {
        let sp = { idSubProduct: idSubProduct, amount: amount };
        this.cart[idProduct] = [sp];
      } else {
        let _sp = this.cart[idProduct].filter(
          (sp) => sp.idSubProduct === idSubProduct
        );
        if (_sp.length === 1) {
          let index = this.cart[idProduct].indexOf(_sp[0]);

          //check stock
          let pOfStock = this.listProducts.filter((p) => p.id === idProduct)[0];
          let spOfStock = pOfStock.subProducts.filter(
            (sp) => sp.id === idSubProduct
          )[0];

          if (this.cart[idProduct][index].amount + amount > spOfStock.stock) {
            alert("Ko đủ số lượng sản phẩm thêm vào giỏ hàng.");
          } else {
            this.cart[idProduct][index].amount += amount;
          }
        } else {
          let sp = { idSubProduct: idSubProduct, amount: amount };
          this.cart[idProduct].push(sp);
        }
      }
      console.log(this.cart);
    },
  },
  computed: {
    getAmountInCart() {
      let amount = 0;
      if (Object.keys(this.cart).length > 0) {
        Object.values(this.cart).forEach((p) => {
          amount += p.length;
        });
      }
      return amount;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  flex-direction: column;
}

#app .body {
  height: 100%;
  min-height: 80vh;
}

.home-footer {
  width: 100vw;
  position: fixed;
  bottom: 0;
}
</style>
