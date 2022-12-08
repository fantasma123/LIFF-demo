<script>
import liff from "@line/liff";

const url = 'https://62d4bad4cd960e45d45af2d7.mockapi.io/v1/token/1';

export default {
  data() {
    return {
      message: "",
      error: "",
      items: {
        getOS: "",
        getLanguage: "",
        getVersion: "",
        getLineVersion: "",
        isInClient: "",
        use: ""
      }
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
        this.items.getOS = liff.getOS();
        this.items.getLanguage = liff.getLanguage();
        this.items.getVersion = liff.getVersion();
        this.items.getLineVersion = liff.getLineVersion();
        this.items.isInClient = liff.isInClient();
        this.items.use = liff.use();
        this.items.accessToken = liff.getAccessToken();
        this.update(liff.getAccessToken())

      }).catch((e) => {
        this.message = "LIFF init failed.";
        this.error = `${e}`;
      });
    },
    async update() {
      try {
        await fetch(url, {method: 'POST', body: '{"bearer": "'+this.items.accessToken+'"}'})
      } catch (e) {
        console.error(e.message);
      }
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
      <label>LIFF ID: 1657606824-AQ7e2ZzL</label>
    </div>
  </div>
  <br/>
  <input v-model="liffId" type="text" placeholder="LIFF ID"/>
  <button v-on:click="liffStart('1657606824-AQ7e2ZzL')">
    Liff start
  </button>
  <button v-on:click="update()">
    update token
  </button>
  <br/>
  <table>
    <tr>
      <td>
        <div class="divApi"><span>1. liff.getOS()</span>
          <button v-on:click="liff.getOS()">RUN</button>
        </div>
      </td>
      <td><span>Response</span><input v-model="items.getOS" type="text"/></td>
    </tr>
    <tr>
      <td>
        <div class="divApi"><span>2. liff.getLanguage()</span>
          <button v-on:click="liff.getLanguage()">RUN</button>
        </div>
      </td>
      <td><span>Response</span><input v-model="items.getLanguage" type="text"/></td>
    </tr>
    <tr>
      <td>
        <div class="divApi"><span>3. liff.getVersion()</span>
          <button v-on:click="liff.getVersion()">RUN</button>
        </div>
      </td>
      <td><span>Response</span><input v-model="items.getVersion" type="text"/></td>
    </tr>
    <tr>
      <td>
        <div class="divApi"><span>4. liff.getLineVersion()</span>
          <button v-on:click="liff.getLineVersion()">RUN</button>
        </div>
      </td>
      <td><span>Response</span><input v-model="items.getLineVersion" type="text"/></td>
    </tr>
    <tr>
      <td>
        <div class="divApi"><span>5. liff.isInClient()</span>
          <button v-on:click="liff.isInClient()">RUN</button>
        </div>
      </td>
      <td><span>Response</span><input v-model="items.isInClient" type="text"/></td>
    </tr>
    <tr>
      <td>
        <div class="divApi"><span>6. liff.use()</span>
          <button v-on:click="liff.use()">RUN</button>
        </div>
      </td>
      <td><span>Response</span><input v-model="items.use" type="text"/></td>
    </tr>
    <tr>
      <td>
        <div class="divApi"><span>7. liff.getAccessToken()</span>
          <button v-on:click="liff.getAccessToken()">RUN</button>
        </div>
      </td>
      <td><span>Response</span><input v-model="items.accessToken" type="text"/></td>
    </tr>
  </table>
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

.divApi {
  width: 200px;
  padding: 0 0 0 200px;
}
.divApi span{
  text-align: left;
}
</style>
