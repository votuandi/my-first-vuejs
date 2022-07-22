<template>
  <div id="product-detail-container">
    <div class="product">
      <div class="product-image">
        <div class="image">
          <img :src="getImgProductPath" alt="" />
        </div>
      </div>
      <div class="product-content">
        <h3 class="title">{{ product.title }}</h3>
        <p class="brand">Thương hiệu: {{ product.brand }}</p>
        <p class="quantity" v-if="getSPQuantity > 0">
          Còn lại: {{ getSPQuantity }} Sản phẩm
        </p>
        <p class="quantity" v-if="getSPQuantity <= 0">Sản phẩm đã hết hàng</p>

        <div class="wrapper-price">
          <div class="final-price">{{ formatFinalPrice }}</div>
          <div class="origin-price">{{ formatOriginalPrice }}</div>
          <div class="sale-price">-{{ product.saleOff * 100 }}%</div>
        </div>
        <div class="wrapper-sp">
          <div class="text">Chọn mẫu</div>
          <div class="list-sp">
            <p class="sp-text">{{ getSPTitle }}</p>
            <ul>
              <li
                v-for="(sp, index) in product.subProducts"
                :class="classActive(index)"
                :key="index"
                v-on:click="handleClickSP($event, index)"
              >
                <img :src="getImgSPPath(sp.id)" :key="index" />
              </li>
            </ul>
          </div>
        </div>
        <add-product-counter v-on:getAddProductCount="getAddProductCount" />
      </div>
    </div>
    <div class="description">
      <ul class="extra-info">
        <li v-for="(info, in4index) in this.product.sumary" :key="in4index">
          {{ info }}
        </li>
      </ul>
      <h2>Thông tin chi tiết&nbsp;</h2>
      <div class="description" v-html="this.description"></div>
    </div>
  </div>
</template>

<script>
import AddProductCounter from "./AddProductCounter.vue";
export default {
  name: "product-detail",
  components: {
    AddProductCounter,
  },
  create() {},
  beforeMount() {
    this.getDescription().then((r) => {
      this.description = r;
    });
  },
  props: {
    product: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {
      selectSP: 0,
      description: "",
    };
  },
  methods: {
    handleClickSP(e, index) {
      this.selectSP = index;
    },
    classActive(index) {
      return {
        active: this.selectSP === index,
      };
    },
    getImgSPPath(id) {
      return require(`@/assets/img/${id}.jpg`);
    },
    async getDescription() {
      let filePath = `/description/${this.product.id}.txt`;
      let description = "aaa";

      await fetch(filePath)
        .then((response) => response.text())
        .then((data) => {
          description = data;
          return description;
        });

      return description;
    },
    getAddProductCount(amountAddedProduct) {
      if (amountAddedProduct > this.product.subProducts[this.selectSP].stock) {
        alert("Ko đủ số lượng sản phẩm thêm vào giỏ hàng.");
      } else {
        // alert(`Đã thêm ${amountAddedProduct} sản phẩm vào giỏ hàng`);
        this.$emit(
          "getAddedProduct",
          this.product.id,
          this.product.subProducts[this.selectSP].id,
          amountAddedProduct
        );
      }
    },
  },
  computed: {
    getImgProductPath() {
      return require(`@/assets/img/${
        this.product.subProducts[this.selectSP].id
      }.jpg`);
    },
    getSPQuantity() {
      return this.product.subProducts[this.selectSP].stock;
    },
    getSPTitle() {
      return this.product.subProducts[this.selectSP].title;
    },
    formatOriginalPrice() {
      return new Intl.NumberFormat("de-DE", {
        style: "currency",
        currency: "VND",
      }).format(this.product.originPrice);
    },
    formatFinalPrice() {
      let number = (1 - this.product.saleOff) * this.product.originPrice;
      return new Intl.NumberFormat("de-DE", {
        style: "currency",
        currency: "VND",
      }).format(number);
    },
  },
  watch: {
    product() {
      console.log(this.product);
      this.selectSP = this.product.defaultSubProduct;
    },
  },
};
</script>

<style>
#product-detail-container {
  padding: 15px 10px 10px 20%;
}

img {
  max-width: 100%;
  min-width: 50%;
}
.product {
  max-width: 700px;
  display: flex;
  margin-top: 20px;
  position: relative;
}
.product .product-image {
  flex-basis: 40%;
  max-width: 40%;
  padding-right: 30px;
}
.product .product-content {
  flex-basis: 60%;
}
.product-image .image {
  width: 100%;
  border: 1px solid #ccc;
}
.product-image .image img {
  display: block;
}
.title {
  margin-top: 0;
  color: #408dda;
  text-decoration: none;
  font-size: 24px;
}
.cart {
  display: inline-block;
  padding: 15px 10px;
  border: 1px solid #ccc;
}
.wrapper-price .final-price {
  color: #f57224;
  font-weight: 700;
  font-size: 22px;
  margin-bottom: 5px;
}
.wrapper-price .origin-price,
.wrapper-price .sale-price {
  display: inline-block;
  font-size: 14px;
  color: #ccc;
  vertical-align: middle;
}
.wrapper-price .sale-price {
  color: #333;
  margin-left: 5px;
  padding: 3px;
  font-size: 10px;
  border-radius: 5px;
  border: 1px solid #f57224;
}

.wrapper-sp {
  display: flex;
  margin-top: 15px;
  padding-top: 15px;
  border-top: 1px solid #eee;
}
.wrapper-sp .text {
  width: 100px;
  min-width: 100px;
}

.wrapper-sp p {
  margin-top: 0;
  margin-bottom: 10px;
  font-weight: bold;
  color: #f57224;
}
.list-sp ul {
  list-style-type: none;
  display: flex;
  margin: 0;
  padding: 0;
}
.list-sp ul li {
  width: 40px;
  height: 40px;
  border: 1px solid #ccc;
  margin-right: 20px;
}
.list-sp ul li:hover,
.list-sp ul li.active {
  border-color: #f57224;
}

.description {
  max-width: 100%;
  max-height: 100%;
}
.description .extra-info {
  padding-left: 0;
  list-style-position: inside;
}
.add-to-cart {
  border: 1px solid #ffb916;
  background: #ffb916;
  color: #fff;
  display: block;
  padding: 12px 30px;
  margin-top: 30px;
  text-transform: uppercase;
  font-size: 16px;
  cursor: pointer;
}

.description {
  height: 500px;
  width: 80vw;
}

@media only screen and (max-width: 750px) {
  #product-detail-container {
    padding: 10px 10px 10px 10px;
    align-items: center;
  }
  .product {
    flex-wrap: nowrap;
    flex-direction: column;
  }
  .product .product-image {
    flex-basis: 100%;
    max-width: 100%;
    min-width: 60%;
    padding-right: 0px;
    align-items: center;
  }
  .product .product-content {
    flex-basis: 60%;
  }
  .product-image .image {
    width: 100%;
    border: 1px solid #ccc;
  }
  .product-image .image img {
    display: block;
  }
}
</style>