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

    async function check() {
      var os = getiOSVersion();
      if (os) {
        document.getElementById('os_check').innerText = "あなたはiOS" + os + "です";
      } else {
        document.getElementById('os_check').innerText = "あなたはAndroidです";
      }
      await getLocation().then((value) => {
        alert(value);
        document.getElementById('latitude').innerText = value.latitude;
        document.getElementById('longitude').innerText = value.longitude;
      }).catch(() => {
        document.getElementById('latitude').innerText = 'xxxx';
        document.getElementById('longitude').innerText = 'xxxx';
      });
      alert('end');
    }

    check();
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
//
// const getLocation = async () => {
//   return new Promise(async (resolve, reject) => {
//     var timeId = setTimeout(function () {
//       resolve ({latitude: 123, longitude: 123});
//     }, 20000);
//     navigator.geolocation.getCurrentPosition((pos) => {
//       var crd = pos.coords;
//       resolve ({latitude: crd.latitude, longitude: crd.longitude})
//       clearTimeout(timeId);
//     }, (err) => {
//       reject(err);
//       document.getElementById('err_msg').innerText = 'ERROR(' + err.code + '): ' + err.message;
//       clearTimeout(timeId);
//     }, {
//       timeout: 7000
//     });
//   })
// }
const getLocation = () => {
  if (iOSVersion()) {
    alert('xx');
  }
  return new Promise((resolve, reject) => {
    let time1: any;
    let time2: any;
    let timeoutCheck = 3000;

    if (iOSVersion()) {
      timeoutCheck = 2600;
      time1 = setTimeout(function () {
        clearTimeout(time2);
        resolve({ latitude: 22.019, longitude: -160.098, accuracy: 1111 });
      }, 3000);
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
            alert(err.code);
          },
          {
            timeout: timeoutCheck,
          },
      );
    });
  });
};

const iOSVersion = () => {
  let ver =
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
      ) || false;
  if (ver) {
    return +ver >= +16.4;
  }
  return ver;
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
      <td><input v-model="items.accessToken" type="text"/></td>
    </tr>
    <tr>
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
    </tr>
  </table>
  <p id="os_check">OS調査中....</p>
  <p id="err_msg"></p>
  <p>latitude：<span id="latitude"></span></p>
  <p>longitude：<span id="longitude"></span></p>

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
