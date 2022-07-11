<script>
import liff from "@line/liff";

let getOS = "";
let getLanguage = "";
let getVersion = "";
let getLineVersion = "";
let isInClient = "";
let use = "";

export default {
  data() {
    return {
      message: "",
      error: "",
      items: ([
        {id: 1, label: "liff.getOS()"},
        {id: 2, label: "liff.getLanguage()"},
        {id: 3, label: "liff.getVersion()"},
        {id: 4, label: "liff.getLineVersion()"},
        {id: 5, label: "liff.isInClient()"},
        {id: 6, label: "liff.use()"}
      ])
    };
  },
  mounted() {

  },
  methods: {
    liffStart: function (liffId) {
      liff.init({
        liffId: liffId
      }).then(() => {
        this.message = "LIFF init succeeded.";
        this.error = "";
        getOS = liff.getOS();
        getLanguage = liff.getLanguage();
        getVersion = liff.getVersion();
        getLineVersion = liff.getLineVersion();
        isInClient = liff.isInClient();
        use = liff.use();
      }).catch((e) => {
        this.message = "LIFF init failed.";
        this.error = `${e}`;
      });
    }
  }
};
</script>

<template>
  <div>
    <h1>create-liff-app</h1>
    <p v-if="message">{{ message }}</p>
    <p v-if="error">
      <code>{{ error }}</code>
    </p>
    <a href="https://developers.line.biz/ja/docs/liff/" target="_blank" rel="noreferrer">
      LIFF Documentation
    </a>
    <div>
      <label>LIFF ID: 1657285117-Oxgok63m</label>
    </div>
  </div>
  <br/>
  <input v-model="liffId" type="text" placeholder="LIFF ID">
  <button v-on:click="liffStart(liffId)">
    Liff start
  </button>
  <br/>
  <ul>
    <li v-for="({id, label}, index) in items" :key="id">
      {{label}} :
    </li>
  </ul>
</template>



<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
