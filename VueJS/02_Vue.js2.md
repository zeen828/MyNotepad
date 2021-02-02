# Vue.js-開始

## 安裝
### NPM 更新
```
npm install -g npm
npm -v
```

### Vue-cli 安裝
```
npm install -g @vue/cli
vue -V
```

### Vue-cli 升級
```
npm update -g @vue/cli
vue -V
```

### 建立專案
```
vue create Project
```

#### 建立專案問答:
```
Vue CLI v4.5.9
? Please pick a preset:
  Default ([Vue 2] babel, eslint)                  >> 預設是基本開的Babel + ESLint preset
  Default (Vue 3 Preview) ([Vue 3] babel, eslint)  >> 預設是基本開的Babel + ESLint preset
> Manually select features                         >> 自己挑選
```

#### 自選建議選項
```
? Check the features needed for your project:
 (*) Choose Vue version
 (*) Babel
 ( ) TypeScript                         >> 支持使用 TypeScript 書寫源碼
 ( ) Progressive Web App (PWA) Support  >> PWA 支持
 (*) Router                             >> 支持 vue-router
 (*) Vuex                               >> 支持 vuex
 (*) CSS Pre-processors                 >> 支持 CSS 預處理器
>(*) Linter / Formatter                 >> 支持代碼風格檢查和格式化
 ( ) Unit Testing                       >> 支持單元測試
 ( ) E2E Testing                        >> 支持 E2E 測試
```

#### 安裝版本?
```
? Choose a version of Vue.js that you want to start the project with
> 2.x
  3.x (Preview)
```

#### 路由
```
? Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n) Y
```
Y:http://localhost:8080/about

N:http://localhost:8080/#/about

#### CSS套件選擇
```
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default):
  Sass/SCSS (with dart-sass)
> Sass/SCSS (with node-sass)
  Less
  Stylus
```

#### 規則
```
? Pick a linter / formatter config:
  ESLint with error prevention only
  ESLint + Airbnb config
> ESLint + Standard config
  ESLint + Prettier
```

#### 執行時間
```
? Pick additional lint features: (Press <space> to select, <a> to toggle all, <i> to invert selection)
>(*) Lint on save
 ( ) Lint and fix on commit
```

#### 設定檔存在哪?
```
? Where do you prefer placing config for Babel, ESLint, etc.?
  In dedicated config files
> In package.json
```

#### 是否存為下次專案預設
```
? Save this as a preset for future projects? (y/N) N
```

#### 安裝結果
```
Vue CLI v4.5.10
? Please pick a preset: Manually select features
? Check the features needed for your project: Choose Vue version, Babel, Router, Vuex, CSS Pre-processors, Linter
? Choose a version of Vue.js that you want to start the project with 2.x
? Use history mode for router? (Requires proper server setup for index fallback in production) Yes
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): Sass/SCSS (with node-sass)
? Pick a linter / formatter config: Standard
? Pick additional lint features: Lint on save
? Where do you prefer placing config for Babel, ESLint, etc.? In package.json
? Save this as a preset for future projects? No
```

---

## 環境變數
### 環境變數設定解說
```
.env                # loaded in all cases.                            (全域設定,不會記錄在Git)
.env.local          # loaded in all cases, ignored by git.            (全域設定,會記錄在Git的)
.env.[mode]         # only loaded in specified mode.                  (mode設定,不會記錄在Git)
.env.[mode].local   # only loaded in specified mode, ignored by git.  (mode設定,會記錄在Git的)
```

### 設定內容範例
```
NODE_ENV=development
VUE_APP_SECRET=envdevelopment
```
PS.前墜VUE_APP必要

### 使用範例
```js
console.log(process.env.VUE_APP_NOT_SECRET_CODE)
```

### 配置設置
```json
  "scripts": {
    "serve": "vue-cli-service serve",
    "serve-dev": "vue-cli-service serve --mode dev",
    "serve-test": "vue-cli-service serve --mode test",
    "serve-pro": "vue-cli-service serve --mode pro",
    "build": "vue-cli-service build",
    "build-dev": "vue-cli-service build --mode dev",
    "build-test": "vue-cli-service build --mode test",
    "build-pro": "vue-cli-service build --mode pro",
    "lint": "vue-cli-service lint"
  },
```
改package.json

### 運行指令
```
npm run serve
npm run serve-dev
npm run serve-test
npm run serve-pro
npm run build
npm run build-dev
npm run build-test
npm run build-pro
```

[參考](https://cli.vuejs.org/guide/mode-and-env.html#example-staging-mode)

---

## 基礎
### 基礎函數
```
name             這個元件實體的名稱
components       載入其他的元件到這個元件實體內
mixins           載入混合模式元件
props            讀取外部傳入的屬性
                 type 這個變數的型別，你也可以不指定，就照單全收。
                 required 規定這個屬性是不是必要存在，僅接受布林值。
                 default 設定該屬性值的預設值，必須注意一點，如果是物件，必須要用函數的方法來返回預設值。
                 validator 自定義的型別判斷。
data()           設定預設值
inheritAttrs     設定該元件是否繼承相關屬性，僅接受布林值
directives       元件自身的自定義指令
filters          元件自身定義的過濾器
methods          設定元件內部方法
computed         設定計算屬性
watch            監聽工具
```

### 與生命週期有關的函數與呼叫順序
```
beforeCreate()   元件建立之前。
created()        元件建立完成。
beforeMount()    元件被放入 DOM 結構樹之前。
mounted()        元件放入 DOM 結構樹之後。
beforeUpdate()   元件實體更新之前。
updated()        元件實體更新之後。
beforeDestroy()  元件被銷毀（從 DOM 結構樹移除）之前。
destroyed()      元件被銷毀（從 DOM 結構樹移除）之後。
```

---

## Vuex - 狀態管理
### 建構新vuex
```js
export default {
  namespaced: true,
  // 用來資料共享資料儲存
  state: {
    accessToken: '',
    signature: '',
    debug: false
  },
  // 用來註冊改變資料狀態
  // this.$store.commit('mutations function name')
  mutations: {
    setAccessToken (state, val) {
      state.accessToken = val
    },
    setSignature (state, val) {
      state.signature = val
    },
    setDebug (state, val) {
      state.debug = val
    }
  },
  // 用來對共享資料進行過濾操作
  getters: {
    getAccessToken (state) {
      return state.accessToken
    },
    getSignature (state) {
      return state.signature
    },
    getDebug (state) {
      return state.debug
    }
  },
  // 解決非同步改變共享資料
  // 建議透過這這邊執行邏輯運算後呼叫mutations
  // this.$store.dispatch('actions function name')
  actions: {
    ready (context) {
      console.log('Vues.store.model.action.ready')
    }
  }
}

```
建立/src/store/modules/user.js

### 擴展模組
```js
import Vue from 'vue'
import Vuex from 'vuex'

// module
import apiUser from './modules/user'

Vue.use(Vuex)

export default new Vuex.Store({
  // 用來資料共享資料儲存
  state: {
    debug: false
  },
  // 用來註冊改變資料狀態
  mutations: {
    setDebug (state, val) {
      state.debug = val
    }
  },
  // 用來對共享資料進行過濾操作
  getters: {
    getDebug (state) {
      return state.debug
    }
  },
  // 解決非同步改變共享資料
  actions: {
    ready: function ({ commit, dispatch, rootState, state }) {
      console.log('Vues.store.action.ready')
    }
  },
  // store的子模組，為了開發大型專案，方便狀態管理而使用的
  modules: {
    apiUser
  }
})
```
修改/src/store/index.js

### 使用範例
```js
export default {
  name: 'App',
  computed: {
    userDebug: {
      get () {
        // 取modules的值
        return this.$store.getters['module/getDebug']
      },
      set (val) {
        // 執行mutations
        this.$store.commit('module/setDebug', val)
      }
    }
  },
  mounted () {
    // 取根的值
    this.$store.getters.getDebug
    // 執行actions
    this.$store.dispatch('module/ready'),
  }
}
```
App.vue中使用範例

```js
  actions: {
    ready (context) {
      // 取本身的值
      var val = context.getters.getDebug
      // 取根的值
      var valRoot = context.rootGetters.getDebug
      // 取modules的值
      var valModule = context.rootGetters['storeFintechClient/getDebug']
      // 執行根的mutations
      context.commit('setDebug', 'false', { root: true })
      // 執行module的mutations
      context.commit('storeFintechClient/setDebug', 'false', { root: true })
      // 執行根的actions
      context.dispatch('ready', 'false', { root: true })
      // 執行module的actions
      context.dispatch('storeFintechClient/ready', 'false', { root: true })
    },
  }
```
src\store\index.js中使用範例

```js
import Store from '@/store/index.js'
// 取根的值
var valRoot = Store.getters.getDebug
// 取modules的值
var valModule = Store.getters['storeFintechClient/getDebug']
// 執行根的mutations
Store.commit('setDebug', false)
// 執行module的mutations
Store.commit('storeFintechClient/setDebug', false)
// 執行根的actions
Store.dispatch('ready')
// 執行module的actions
Store.dispatch('storeFintechClient/ready')
```
api.js整合中使用範例

> ### 同步非同步
> #### 同步 (一個動作執行完，才作下一個動作)
> ```js
> import ajaxFintechUser from '@/api/fintech/user'
>   methods: {
>     async ready () {
>       var that = this
>       // 等待排隊執行
>       try {
>         // call api one
>         const one = await ajaxFintechUser.getToken(that.account, that.password)
>         console.table('第一個 回傳結果', one)
>         window.localStorage.setItem('access_token', one.data.data.access_token)
>         // call api two
>         const two = await ajaxFintechUser.getRead()
>         console.table('第二個 回傳結果', two)
>       } catch (error) {
>         throw new Error(error)
>       }
>     }
>   }
> ```
> 
> #### 非同步 (不需要等待上一個動作完成，才作下一個動作)
> ```js
> import ajaxFintechClient from '@/api/fintech/client'
> import ajaxBetClient from '@/api/bet/client'
>   methods: {
>     ready () {
>       var that = this
>       // 同時執行
>       axios.all([
>         // call api one
>         ajaxFintechClient.getToken(),
>         // call api two
>         ajaxBetClient.getToken()
>       ]).then(axios.spread((one, two) => {
>         // axios 回傳的資料在 data 屬性
>         console.table('第一個 回傳結果', one)
>         // fetch 資料可以先在 function 內作 json()
>         console.table('第二個 回傳結果', two)
>       })).catch((err) => {
>         console.error(err)
>       })
>     }
>   }
> ```

[參考](https://vuex.vuejs.org/zh/)

---

## axios - ajax用
### 安裝
```
npm install --save axios
```

### 建立基礎架構
```js
import axios from 'axios'

export default () => {
  /**
   * axios實利
   */
  var ajaxBase = axios.create({
    // 判斷是否垮域,用來解決CORS
    withCredentials: false,
    baseURL: process.env.VUE_APP_BET_API_PROTOCOL + '://' + process.env.VUE_APP_BET_API_DOMAIN + '/' + process.env.VUE_APP_BET_API_VERSION,
    // 表頭
    headers: {
      'Content-Type': 'application/json',
      Accept: 'application/json',
      'Accept-Language': process.env.VUE_APP_LANGUAGE,
      'X-Timezone': process.env.VUE_APP_TIMEZONE,
      Authorization: 'Bearer ' + window.localStorage.getItem('access_token')
    },
    // 請求超時1000毫秒(1秒)
    timeout: 3000,
    // onUploadProgress(progressEvt) {},
    // onDownloadProgress(progressEvt) {},
    // 資料發送至伺服器前，可作資料處理
    // 陣列中最後的函式必須返回字串、ArrayBuffer、Stream
    transformRequest: [
      function (data, headers) {
        return data
      }
    ]
    // 在進入 then / catch 前可作資料處理
    // transformResponse: [function(data) {return data;}]
  })

  return ajaxBase
}
```
建立目錄和檔案src/api/base.js

### 建立API用
```js
import Api from '@/api/bet/base'

export default {
  getToken () {
    console.log('api.bet.client.js > getToken()')
    const formData = new FormData()
    formData.append('client_id', process.env.VUE_APP_BET_CLIENT_ID)
    formData.append('client_secret', process.env.VUE_APP_BET_CLIENT_SECRET)
    // formData.append('client_id', 'ebb3c65c371144d0840149d5776f914d')
    // formData.append('client_secret', '5f0f4f5202125160f02dcec44e7cfab6')
    return Api().post('/auth/token', formData)
    // return Api().post('http://localhost:8000/api/v1/auth/token', formData)
  }
}
```
建立目錄和檔案src/api/api.js

### 使用範例
```js
import ajaxApi from '@/api/fintech/api'

export default {
  name: 'App',
  beforeCreate () {},
  created () {},
  mounted () {
    console.log('App.vue > mounted ()')
    this.ready()
  },
  beforeDestroy () {},
  destroyed () {},
  methods: {
    ready () {
      var that = this
      that.callApi()
    },
    callApi () {
      var that = this
      ajaxApi.getToken().then(function (result) {
        console.log(result)
        if (result.status === 200 && result.request.readyState === 4) {
          // 打API成功處理動作
          const apiData = result.data
          if (apiData !== null && apiData.success === true) {
            console.log(apiData)
            // 回傳成功處理動作
          }
        }
      })
    }
  }
}
```
App.vue中使用範例

[參考](https://developer.aliyun.com/mirror/npm/package/vue-axios)

---

## i18n - 多國語系
### 安裝
```
npm install vue-i18n
```

### 設定
```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

// i18n - 多國語系
import VueI18n from 'vue-i18n'// 引入 Vue I18n
import zh from './i18n/zh'// 存放中文語系檔
import en from './i18n/en'// 存放英文語系檔

Vue.use(VueI18n)

// 預設使用的語系
let locale = 'en'

// 檢查 localStorage 是否已有保存使用者選用的語系資訊
if (localStorage.getItem('footmark-lang')) {
  locale = localStorage.getItem('footmark-lang')
  store.commit('setLang', locale)
} else {
  store.commit('setLang', locale)
}

const i18n = new VueI18n({
  locale: locale,
  messages: {
    zh: zh,
    en: en
  }
})

Vue.config.productionTip = false

new Vue({
  router,
  store,
  i18n,
  render: h => h(App)
}).$mount('#app')
```
修改/src/main.js

### 建置語系檔
```json
{
    "title": "MIS footmark",
    "description": "IT learn",
    "test": "Test"
}
```
建立目錄和檔案src\i18n\en.json

```json
{
    "title": "MIS 腳印",
    "description": "記錄 IT 學習的軌跡",
    "test": "測試"
}
```
建立目錄和檔案src\i18n\zh.json

```vue
<template>
  <div class="lang">
    <label v-for="(item, index) in optionsLang" v-bind:key="index">
      <input type="radio" v-model="$store.state.lang" :value="item.value" v-on:change="setLang(item.value)"> {{ item.text }}
    </label>
    <h1>{{ $t('title')}}</h1>
    <h2>{{ $t('description')}}</h2>
  </div>
</template>

<script>
export default {
  name: 'Lang',
  data () {
    return {
      optionsLang: [
        { text: '中文', value: 'zh' },
        { text: 'English', value: 'en' }
      ]
    }
  },
  methods: {
    setLang (value) {
      this.$store.commit('setLang', value)
      this.$i18n.locale = value
      localStorage.setItem('footmark-lang', value)
    }
  }
}
</script>
```
建立目錄和檔案src\components\html\Lang.vue

### 使用範例
```vue
<template>
  <HtmlLang/>
</template>

<script>
import HtmlLang from '@/components/html/Lang.vue'

export default {
  name: 'App',
  components: {
    HtmlLang
  }
}
</script>
```

### 替換變數
```json
{
    "page": "This is an {name} page."
}
```

```html
<h2>{{ $t('page', {name: "首頁"})}}</h2>
```

---

## component slot(組件留白)
### 組件
```vue
<template>
  <div class="test">
    <h1>我是測試</h1>
    <slot name='header'></slot>
    <slot name='content'></slot>
    <slot></slot>
    <slot name='footer'></slot>
  </div>
</template>

<script>
export default {
  name: 'Test'
}
</script>
```

### 使用
```vue
<template>
  <div class="app">
    <Test>
      <template v-slot:footer>我要指定name是footer的slot內容</template>
      <template v-slot:header>我要指定name是header的slot內容</template>
      <template>我要指定沒有name(default)的slot內容</template>
      <template v-slot:content>我要指定name是content的slot內容</template>
    </Test>
  </div>
</template>

<script>
import Test from '@/components/test/Test.vue'
export default {
  name: 'App',
  components: {
    Test
  }
}
</script>
```


---

## Vue Material - 樣板
### 安裝
```
npm install vue-material --save
```

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

// Vue Material - 樣板
import VueMaterial from 'vue-material'
import 'vue-material/dist/vue-material.min.css'
import 'vue-material/dist/theme/default.css'

Vue.use(VueMaterial)

Vue.config.productionTip = false

new Vue({
  router,
  store,
  i18n,
  render: h => h(App)
}).$mount('#app')
```
修改/src/main.js

[樣板官方](https://vuematerial.io/)

---

## Mixins(共用函式)-->(3.0建議改用Composition API)
```javascript
export default {
  data: () => ({
    debug: false
  }),
  methods: {
  }
}
```
\src\mixins\customFun.js

```javascript
// 自訂
import '@/mixins/customFun.js'
```
\src\main.js

[參考](https://v3.vuejs.org/guide/mixins.html)

---

## Cookie
### 安裝
```
npm install vue-cookies --save
```

#### 2.x
```javascript
import Vue from 'vue'
import VueCookies from 'vue-cookies'
Vue.use(VueCookies)
```
\src\main.js

#### 3.0
```javascript
//import { createApp } from 'vue'
//import App from './App.vue'
//import router from './router'
//import store from './store'
import VueCookies from 'vue-cookies'// cookie

createApp(App).use(store).use(router).use(VueCookies).mount('#app')

// 3.0要套用進去變成這樣註解的吊飾原本程式不用動的部份
```
\src\main.js

### 用法
```
// 元件中直接使用 來設定cookie
this.$cookies.set('test', 'val', 60 * 60)
// 元件中獲取方式
this.$cookies.get('test') 
// 刪除 cookie
this.$cookies.remove('test')
// 檢查key
this.$cookies.isKey('test')
// 獲取所有cookie名稱
this.$cookies.keys()
```

[參考](https://blog.csdn.net/qq_35250826/article/details/106111768)

---

##