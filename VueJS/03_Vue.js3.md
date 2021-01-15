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
 ( ) Vuex                               >> 支持 vuex
 (*) CSS Pre-processors                 >> 支持 CSS 預處理器
>(*) Linter / Formatter                 >> 支持代碼風格檢查和格式化
 ( ) Unit Testing                       >> 支持單元測試
 ( ) E2E Testing                        >> 支持 E2E 測試
```

#### 安裝版本?
```
? Choose a version of Vue.js that you want to start the project with
  2.x
> 3.x (Preview)
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
? Check the features needed for your project: Choose Vue version, Babel, Router, CSS Pre-processors, Linter
? Choose a version of Vue.js that you want to start the project with 3.x (Preview)
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
package.json

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

## Composition API
Vue.js 3.0用Composition API替代了Vuex與Mixins的用法

```
npm install --save @vue/composition-api
```
