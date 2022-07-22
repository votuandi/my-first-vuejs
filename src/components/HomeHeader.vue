<template>
  <div class="my-header">
    <a href="#home" class="logo" v-on:click="backHome">Minwy Shop</a>
    <search-bar class="search-bar" v-on:onHearderSearch="getSearchBarContent" />
    <a href="#cart" class="cart"
      ><font-awesome-icon icon="cart-shopping" color="#fff" /> ({{
        amountInCart
      }})</a
    >
  </div>
</template>

<script>
import SearchBar from "./SearchBar.vue";
import { library } from "@fortawesome/fontawesome-svg-core";
import { faCartShopping } from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
library.add(faCartShopping);
export default {
  name: "home-header",
  components: {
    SearchBar,
    FontAwesomeIcon,
  },
  props: {
    amountInCart: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {};
  },
  methods: {
    getSearchBarContent(searchContent) {
      this.$emit("searchBySearachBar", searchContent);
    },
    backHome() {
      this.$emit("showHomePage");
    },
  },
};
</script>

<style>
.my-header {
  overflow: hidden;
  background-color: #a7c7e7;
  padding: 20px 10px;
  box-sizing: border-box;
  align-items: center;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

@media only screen and (max-width: 600px) {
  .my-header {
    padding: 5px 5px 5px 5px;
    flex-wrap: nowrap;
    flex-direction: column;
  }

  .my-header .search-bar {
    width: 80vh;
    min-width: 200px;
  }
}

.my-header a {
  color: #fff;
  text-align: center;
  padding: 12px;
  text-decoration: none;
  font-size: 18px;
  line-height: 25px;
  border-radius: 4px;
}

.my-header a.logo {
  font-size: 25px;
  font-weight: bold;
  align-self: center;
}

.my-header a:hover {
  background-color: rgba(201, 226, 250, 0.2);
  color: rgba(255, 214, 214, 0.8);
}

.my-header a.active {
  background-color: dodgerblue;
  color: white;
}

.my-header .search-bar {
  width: 40%;
  align-self: center;
}

.my-header a.cart {
  border: 1px solid #fff;
  color: #fff;
  font-size: 16px;
  min-width: 70px;
  /* width: 60px; */
  padding: 1px 1px 1px 1px;
  margin: 10px 10px 10px 10px;
  align-self: center;
  margin-right: 10px;
}
</style>