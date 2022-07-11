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
  </div>
  <br/>
  <input v-model="liffId" type="text" placeholder="LIFF ID">
  <button v-on:click="liffStart(liffId)">
    Liff start
  </button>
  <br/>
  {{liffId}}
</template>

<script>
import liff from "@line/liff";
import {ref} from 'vue'

const liffId = ref("1657285117-Oxgok63m");

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
      }).catch((e) => {
        this.message = "LIFF init failed.";
        this.error = `${e}`;
      });
    }
  }
};
</script>

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
