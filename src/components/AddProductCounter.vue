<template>
  <div class="apc-container">
    <div class="counter">
      <button
        class="btn-counter"
        id="minus-counter"
        v-on:click="changeCount(false)"
      >
        -
      </button>
      <input
        class="inp-counter"
        type="text"
        name="product-counter"
        :value="spCount"
      />
      <button
        class="btn-counter"
        id="plus-counter"
        v-on:click="changeCount(true)"
      >
        +
      </button>
    </div>
    <button class="btn-add2cart" v-on:click="submitCounter">
      Thêm vào giỏ hàng
    </button>
  </div>
</template>

<script>
export default {
  name: "add-product-counter",
  data() {
    return {
      spCount: 1,
    };
  },
  methods: {
    changeCount(isPlus) {
      if (isPlus) {
        this.spCount++;
      } else {
        if (this.spCount > 0) {
          this.spCount--;
        }
      }
    },
    submitCounter() {
      this.$emit("getAddProductCount", this.spCount);
    },
  },
};
</script>

<style>
.apc-container {
  display: inline-block;
  width: 400px;
  align-items: center;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-content: center;
  margin: 20px 0px 20px 0px;
}

@media only screen and (max-width: 400px) {
  .apc-container {
    width: auto;
    flex-wrap: nowrap;
    flex-direction: column;
  }
}

.counter {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
}

.inp-counter {
  min-width: 30px;
  max-width: 50px;
  font-size: 16px;
  font-weight: 700;
  line-height: 30px;
  text-align: center;
  margin: 0 5px 0 5px;
  border: 2px solid#ffb916;
  border-radius: 25%;
}

.btn-counter {
  cursor: pointer;
  display: inline-block;
  vertical-align: top;
  background-color: #ffb916;
  color: #fff;
  width: 30px;
  height: 30px;
  font: 18px/1 Arial, sans-serif;
  font-weight: bolder;
  text-align: center;
  border: 1px solid rgba(255, 255, 255, 0);
  border-radius: 50%;
}

.btn-add2cart {
  border: 1px solid #ffb916;
  background: #ffb916;
  color: #fff;
  display: block;
  padding: 12px 30px;
  text-transform: uppercase;
  font-size: 16px;
  font-weight: bolder;
  cursor: pointer;
  border-radius: 10px;
}
</style>