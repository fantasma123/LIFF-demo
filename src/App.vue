<script>
import liff from "@line/liff";

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
        use: "",
        permissions: "",
        platform: "",
        ver: "",
      }
    };
  },
  mounted() {
    function getiOSVersion() {
      return parseFloat(
          ('' + (/CPU.*OS ([0-9_]{1,5})|(CPU like).*AppleWebKit.*Mobile/i.exec(navigator.userAgent) || [0, ''])[1])
              .replace('undefined', '3_2')
              .replace('_', '.')
              .replace('_', '')
      ) || false;
    }
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

      }).catch((e) => {
        this.message = "LIFF init failed.";
        this.error = `${e}`;
      });
    },
    async permissions() {
      try {
        this.items.permissions = (await navigator?.permissions?.query({
          name: 'geolocation',
        }))?.state;
        await this.ios();
      } catch (e) {
        console.error(e.message);
      }
    },
    async ios() {
      this.items.platform = navigator.platform;
      if (/iP(hone|od|ad)/.test(navigator.platform)) {
        const version = navigator.appVersion.match(/OS (\d+)_(\d+)_?(\d+)?/);
        this.items.ver = parseInt(version[1], 10) + '.' + parseInt(version[2], 10);
        return;
      }
      this.items.ver = '...'
    },
    sample: async function () {
      console.log('a');
      console.log('waiting...')
      await delay(3000);
      console.log('b');
    }
  }
};
const delay = (delayInms) => {
  return new Promise(resolve => {
    setTimeout(resolve, delayInms);
    setTimeout(() => {
      console.log('xx');
      resolve
    }, delayInms + 1000);
  });
};

const getZoom = () => {
  document.getElementById('zoom').innerText = document.documentElement.clientWidth / window.innerWidth;
};
const getLocation = () => {
  alert(iOSVersion());
  return new Promise((resolve, reject) => {
    let time1;
    let time2;
    let timeoutCheck = 3000;
    let timeout1 = 4000;
    let code = 1112;
    if (iOSVersion() != 0) {
      if (iOSVersion() >= 16.4) {
        code = 1111;
        timeout1 = 3000;
        timeoutCheck = 2500;
      }
      time1 = setTimeout(function () {
        clearTimeout(time2);
        resolve({latitude: 22.019, longitude: -160.098, accuracy: code});
      }, timeout1);
    }

    time2 = setTimeout(function () {
      navigator.geolocation.getCurrentPosition(
          (position) => {
            const crd = position.coords;
            if (time1) clearTimeout(time1);
            resolve({
              latitude: crd.latitude,
              longitude: crd.longitude,
              accuracy: crd.accuracy,
            });
          },
          (err) => {
            if (time1) clearTimeout(time1);
            if (err.code != 1 && iOSVersion() != 0) {
              resolve({latitude: 22.019, longitude: -160.098, accuracy: 1113});
            } else {
              reject(err);
            }
          },
          {
            enableHighAccuracy: true,
            timeout: timeoutCheck,
          },
      );
    });
  });
};

const iOSVersion = () => {
  const ver =
      parseFloat(
          (
              '' +
              (/CPU.*OS ([0-9_]{1,5})|(CPU like).*AppleWebKit.*Mobile/i.exec(
                  navigator.userAgent,
              ) || [0, ''])[1]
          )
              .replace('undefined', '3_2')
              .replace('_', '.')
              .replace('_', ''),
      ) || 0;
  return +ver;
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
      <label>LIFF ID: 1657268313-GkmW448O</label>
    </div>
  </div>
  <br/>
  <input v-model="liffId" type="text" placeholder="LIFF ID"/>
  <button v-on:click="liffStart(liffId)">
    Liff start
  </button>
  <button v-on:click="permissions()">
    get permissions
  </button>
  <button v-on:click="sample()">
    check Timeout
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
      <td><span>Response</span></td>
      <td><input id='accessToken' v-model="items.accessToken" type="text"/></td>
    </tr>
<!--    <tr>
      <td><span>permissions</span></td>
      <td><input v-model="items.permissions" type="text"/></td>
    </tr>
    <tr>
      <td><span>platform</span></td>
      <td><input v-model="items.platform" type="text"/></td>
    </tr>
    <tr>
      <td><span>ver</span></td>
      <td><input v-model="items.ver" type="text"/></td>
    </tr>-->
  </table>
  <!--<p id="os_check">OS調査中....</p>
  <p id="err_msg"></p>
  <p>userAgent：<span id="userAgent"></span></p>
  <p>latitude：<span id="latitude"></span></p>
  <p>longitude：<span id="longitude"></span></p>
  <p>zoom: <span id='zoom'></span></p>-->
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

table {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
