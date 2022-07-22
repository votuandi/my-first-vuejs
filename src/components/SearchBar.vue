<template>
  <div class="seach-bar">
    <input
      type="text"
      class="inp-search"
      placeholder="Tìm tên sản phẩm hoặc cái gì cũng được..."
      v-model="inpSearchContent"
    />
    <button class="btn-search" v-on:click="sendSearchContent">
      <font-awesome-icon icon="search" color="#fff" />
    </button>
  </div>
</template>

<script>
import { library } from "@fortawesome/fontawesome-svg-core";
import { faSearch } from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
library.add(faSearch);

export default {
  name: "seach-bar",
  data() {
    return {
      inpSearchContent: "",
    };
  },
  components: {
    FontAwesomeIcon,
  },
  methods: {
    sendSearchContent() {
      var searchContent = {
        text: this.inpSearchContent,
      };
      this.$emit("onHearderSearch", searchContent);
    },
  },
};
</script>

<style>
.seach-bar {
  height: 2.5rem;
  max-width: 60vw;
  min-width: 200px;
  background-color: #fffffa;
  border-radius: 20px;
  border: 1px solid #1e90ff;
  box-sizing: border-box;
  display: flex;
  justify-content: space-between;
  align-items: stretch;
  padding: 0.1875rem;
}

.seach-bar:focus {
  background-color: #96c9fb;
}

.seach-bar .inp-search {
  font-size: 10px;
  width: 90%;
  /* min-width: 36vw; */
  border: 1px solid rgba(255, 255, 255, 0);
  border-radius: 20px;
}

.seach-bar .inp-search:focus {
  outline: none !important;
  background-color: #96c8fb29;
}

.seach-bar .btn-search {
  background-color: #1e90ff;
  color: #fff;
  border: 1px solid #fff;
  border-radius: 20px;
  font-size: 14px;
  font-weight: bold;
  /* min-width: 4vw; */
}

.seach-bar .btn-search:hover {
  background-color: #00bfff;
}
.seach-bar .btn-search:active {
  box-shadow: inset -2px -2px 3px rgba(255, 255, 255, 0.6),
    inset 2px 2px 3px rgba(0, 0, 0, 0.6);
}
</style>