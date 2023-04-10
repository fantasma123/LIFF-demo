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
      }).then(async () => {
        this.message = "LIFF init succeeded.";
        this.error = "";
        this.items.getOS = liff.getOS();
        this.items.getLanguage = liff.getLanguage();
        this.items.getVersion = liff.getVersion();
        this.items.getLineVersion = liff.getLineVersion();
        this.items.isInClient = liff.isInClient();
        this.items.use = liff.use();
        this.items.accessToken = liff.getAccessToken();

        await navigator.geolocation.getCurrentPosition(
            (position) => {
              this.items.latitude = 'done';
              const {latitude, longitude, accuracy} = position.coords;
              this.items.latitude = latitude;
              this.items.longitude = longitude;
              this.items.accuracy = accuracy;
            },
            (err) => {
              this.items.latitude = 'err';
              if (/iP(hone|od|ad)/.test(navigator.platform)) {
                const version = navigator.appVersion.match(/OS (\d+)_(\d+)_?(\d+)?/);
                let ver = '1.1';
                if (version) {
                  ver = parseInt(version[1], 10) + '.' + parseInt(version[2], 10);
                }
                if (+ver >= +16.4) {
                  const latitude = 22.019;
                  const longitude = -160.098;
                  const accuracy = 164;
                  this.items.latitude = latitude;
                  this.items.longitude = longitude;
                  this.items.accuracy = accuracy;
                } else {
                  this.items.latitude = 'xxxx';
                }
              }
            },
            {timeout: 15000},
        );
      }).catch((e) => {
        this.message = "LIFF init failed.";
        this.error = `${e}`;
      });
    },
    async update() {
      try {
        await fetch(url, {method: 'PUT', body: '{"bearer": "Bearer ' + this.items.accessToken + '"}'})
      } catch (e) {
        console.error(e.message);
      }
    },
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
      <label>LIFF ID: 1657475863-1NRbXoX8</label>
    </div>
  </div>
  <br/>
  <input v-model="liffId" type="text" placeholder="LIFF ID"/>
  <button v-on:click="liffStart(liffId)">
    Liff start
  </button>
  <button v-on:click="update()">
    update token
  </button>
  <br/>
  <table>
<!--    <tr>
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
    </tr>-->
    <tr>
      <td>
        <div class="divApi"><span>7. liff.getAccessToken()</span>
          <button v-on:click="liff.getAccessToken()">RUN</button>
        </div>
      </td>
      <td><span>Response</span><input v-model="items.accessToken" type="text"/></td>
    </tr>
    , ,
    <tr>
      <td>
        <div class="divApi"><span>7. latitude</span>
        </div>
      </td>
      <td><span>latitude</span><input v-model="items.latitude" type="text"/></td>
    </tr>
    <tr>
      <td>
        <div class="divApi"><span>8. longitude</span>
        </div>
      </td>
      <td><span>longitude</span><input v-model="items.longitude" type="text"/></td>
    </tr>
    <tr>
      <td>
        <div class="divApi"><span>9. accuracy</span>
        </div>
      </td>
      <td><span>accuracy</span><input v-model="items.accuracy" type="text"/></td>
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

.divApi span {
  text-align: left;
}
</style>
