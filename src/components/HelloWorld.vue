<template>
  <div id="app">
    <div v-bind:class="containerClassName">
      <h1 class="tittle">{{ tittle }}</h1>
    </div>
    <h2>Hello {{ userNames[userName] }}</h2>
    <div class="inp-username-container">
      <span>Hãy nhập tên đăng nhập </span>
      <input type="text" id="inpUserName" v-model="userName" />
    </div>
    <div class="sign-btn-container">
      <input
        class="btn btn-sm sign-btn btn-register"
        id="btnRegister"
        type="button"
        value="Đăng ký"
        v-on:click="changeTab('register')"
      />
      <input
        class="btn btn-sm sign-btn btn-change"
        id="btnChangeName"
        type="button"
        value="Đổi tên"
        v-on:click="changeTab('change')"
      />
    </div>

    <div
      class="inp-container register-container"
      v-if="selectedTab === 'register'"
    >
      <div>
        <span class="txtInput">Tên đăng nhập </span>
        <input
          type="text"
          id="inpUserNameRegister"
          v-model="rgtUsername"
          v-on:change="checkExistedUsername"
        />
        <span
          :style="{ color: 'red', 'margin-left': '10px', display: showStatus }"
          >{{ txtUsernameExisted }}</span
        >
      </div>
      <div style="margin-top: 5px">
        <span class="txtInput" style="margin-top: 5px">Họ và tên </span>
        <input type="text" id="inpUserNameSign" v-model="rgtName" />
      </div>
      <input
        class="btn btn-sm btn-register btn-execute"
        :disabled="isDisabledRegisterBtn"
        type="button"
        value="Đăng ký luôn"
        v-on:click="onRegister"
      />
    </div>

    <div class="inp-container change-container" v-if="selectedTab === 'change'">
      <div>
        <span class="txtInput">Tên đăng nhập </span>
        <input
          type="text"
          id="inpUserNameChange"
          v-model="chUsername"
          v-on:change="checkExistedUsername"
        />
        <span
          :style="{
            color: 'forestgreen',
            'margin-left': '10px',
            display: showStatus,
          }"
          >{{ txtUsernameExisted }}</span
        >
      </div>
      <div style="margin-top: 5px">
        <span class="txtInput" style="margin-top: 5px">Họ và tên </span>
        <input type="text" id="inpNameChange" v-model="chName" />
      </div>
      <input
        class="btn btn-sm btn-change btn-execute"
        :disabled="isDisabledChangeBtn"
        id="btnChange"
        type="button"
        value="Đổi liền"
        v-on:click="onChangeName"
      />
    </div>

    <div>
      <input
        class="btn btn-sm btn-show"
        id="btnShowListUsers"
        type="button"
        value="Danh sách người dùng"
        v-on:click="onShow"
      />
      <ul v-if="isShow">
        <li v-for="(user, key) in userNames" :key="key">
          <div class="list-users">
            <div>{{ key }}</div>
            <div>{{ user }}</div>
            <input type="button" value="Xóa" v-on:click="removeUser(key)" />
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      //Object
      userNames: {
        divt: "Võ Tuấn Dĩ",
        min: "Võ Tuyết Minh",
        kem: "Mèo Kem",
      },

      //txt
      tittle: "My First VueJs App",
      txtUsernameExisted: "Tên đăng nhập tồn tại",

      //var
      selectedTab: "none",
      isExistedUsername: false,
      isDisabledRegisterBtn: true,
      isDisabledChangeBtn: true,
      showStatus: "none",
      isShow: false,

      //style

      //className
      containerClassName: "container",

      //model
      userName: "",
      rgtUsername: "",
      rgtName: "",
      chUsername: "",
      chName: "",
    };
  },

  methods: {
    changeTab(tab) {
      this.selectedTab = tab;
      this.rgtUsername = "";
      this.rgtName = "";
      this.chUsername = "";
      this.chName = "";
    },
    onRegister() {
      this.userNames[this.rgtUsername] = this.rgtName;
      alert(
        `Username ${this.rgtUsername} có tên ${this.rgtName} được tạo thành công!`
      );
      this.rgtUsername = "";
      this.rgtName = "";
    },
    onChangeName() {
      this.userNames[this.chUsername] = this.chName;
      alert(
        `Username ${this.chUsername} đã đổi tên thành ${this.chName} thành công!`
      );
      this.chUsername = "";
      this.chName = "";
    },
    onShow() {
      this.isShow = !this.isShow;
    },
    removeUser(key) {
      delete this.userNames[key];
    },
    userFilter(user) {
      if (user.substr(0, 2) == "Võ") {
        console.log("true");
        return true;
      }
      console.log("false");
      return false;
    },
    checkExistedUsername() {
      if (this.selectedTab === "register") {
        if (this.rgtUsername === "" && this.rgtName === "") {
          this.isDisabledRegisterBtn = true;
        }
        if (this.rgtUsername in this.userNames) {
          this.isExistedUsername = true;
          this.showStatus = "inline-block";
          this.isDisabledRegisterBtn = true;
        } else {
          this.isExistedUsername = false;
          this.showStatus = "none";
          if (this.rgtName !== "") {
            this.isDisabledRegisterBtn = false;
          }
        }
      } else {
        if (this.chUsername === "" && this.chName === "") {
          this.isDisabledChangeBtn = true;
        }
        if (this.chUsername in this.userNames) {
          this.isExistedUsername = true;
          this.showStatus = "inline-block";
          if (this.chName !== "") {
            this.isDisabledChangeBtn = false;
          }
        } else {
          this.isExistedUsername = false;
          this.showStatus = "none";
          this.isDisabledRegisterBtn = true;
        }
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  background-color: deepskyblue;
  color: whitesmoke;
  height: 5vh;
  width: 100vw;
  margin: -10px -8px;
}

.tittle {
  text-align: center;
}

.sign-btn-container {
  margin: 50px 5px;
}

.btn-register {
  border: 0;
  line-height: 2.5;
  padding: 0 20px;
  font-size: 1rem;
  text-align: center;
  color: #fff;
  text-shadow: 1px 1px 1px #000;
  border-radius: 10px;
  background-color: rgba(255, 20, 147, 1);
  background-image: linear-gradient(
    to top left,
    rgba(0, 0, 0, 0.2),
    rgba(0, 0, 0, 0.2) 30%,
    rgba(0, 0, 0, 0)
  );
  box-shadow: inset 2px 2px 3px rgba(255, 255, 255, 0.6),
    inset -2px -2px 3px rgba(0, 0, 0, 0.6);
}

.btn-register:hover {
  background-color: rgba(255, 105, 180);
}

.btn-register:active {
  box-shadow: inset -2px -2px 3px rgba(255, 255, 255, 0.6),
    inset 2px 2px 3px rgba(0, 0, 0, 0.6);
}

.btn-show {
  border: 0;
  line-height: 2.5;
  padding: 0 20px;
  font-size: 1rem;
  text-align: center;
  color: #fff;
  text-shadow: 1px 1px 1px #000;
  border-radius: 10px;
  background-color: rgba(0, 206, 209, 1);
  background-image: linear-gradient(
    to top left,
    rgba(0, 0, 0, 0.2),
    rgba(0, 0, 0, 0.2) 30%,
    rgba(0, 0, 0, 0)
  );
  box-shadow: inset 2px 2px 3px rgba(255, 255, 255, 0.6),
    inset -2px -2px 3px rgba(0, 0, 0, 0.6);
}

.btn-show:hover {
  background-color: rgba(127, 255, 212);
}

.btn-show:active {
  box-shadow: inset -2px -2px 3px rgba(255, 255, 255, 0.6),
    inset 2px 2px 3px rgba(0, 0, 0, 0.6);
}

.btn-execute:disabled {
  background-color: rgba(0, 0, 0, 0.2);
}

.btn-change {
  border: 0;
  line-height: 2.5;
  padding: 0 20px;
  font-size: 1rem;
  text-align: center;
  color: #fff;
  text-shadow: 1px 1px 1px #000;
  border-radius: 10px;
  background-color: rgba(154, 205, 50, 1);
  background-image: linear-gradient(
    to top left,
    rgba(0, 0, 0, 0.2),
    rgba(0, 0, 0, 0.2) 30%,
    rgba(0, 0, 0, 0)
  );
  box-shadow: inset 2px 2px 3px rgba(255, 255, 255, 0.6),
    inset -2px -2px 3px rgba(0, 0, 0, 0.6);
}

.btn-change:hover {
  background-color: rgba(173, 255, 47);
}

.btn-change:active {
  box-shadow: inset -2px -2px 3px rgba(255, 255, 255, 0.6),
    inset 2px 2px 3px rgba(0, 0, 0, 0.6);
}

#btnRegister {
  margin-right: 20px;
}

.txtInput {
  display: inline-block;
  height: 15px;
  width: 100px;
}

.inp-container {
  border-radius: 40px;
  height: 100px;
  width: 500px;
  padding: 30px;
  margin: 10px;
}

.change-container {
  background-color: rgba(144, 238, 144, 0.2);
}

.register-container {
  background-color: rgba(255, 192, 203, 0.2);
}

.btn-execute {
  margin: 20px;

  position: absolute;
  left: 10%;
}

.list-users {
  display: flex;
}

.list-users div {
  width: 10%;
}
</style>
