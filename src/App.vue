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
      error: ""
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
  <input v-model="liffId" type="text" placeholder="LIFF ID"/>
  <button v-on:click="liffStart(liffId)">
    Liff start
  </button>
  <br/>
  <div class="divApi"><span>1. liff.getOS()</span><button v-on:click="liff.getOS()">RUN</button></div>
  <span>Response</span><input v-model="getOS" type="text"/>
  <div class="divApi"><span>2. liff.getLanguage()</span><button v-on:click="liff.getLanguage()">RUN</button></div>
  <span>Response</span><input v-model="getLanguage" type="text"/>
  <div class="divApi"><span>3. liff.getVersion()</span><button v-on:click="liff.getVersion()">RUN</button></div>
  <span>Response</span><input v-model="getVersion" type="text"/>
  <div class="divApi"><span>4. liff.getLineVersion()</span><button v-on:click="liff.getLineVersion()">RUN</button></div>
  <span>Response</span><input v-model="getLineVersion" type="text"/>
  <div class="divApi"><span>5. liff.isInClient()</span><button v-on:click="liff.isInClient()">RUN</button></div>
  <span>Response</span><input v-model="isInClient" type="text"/>
  <div class="divApi"><span>6. liff.use()</span><button v-on:click="liff.use()">RUN</button></div>
  <span>Response</span><input v-model="use" type="text"/>

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
