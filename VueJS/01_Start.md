# Vue.js-開始

## 安裝
```
npm install -g @vue/cli
vue -V
vue create Project
```

### 安裝問答:
```
Vue CLI v4.5.9
? Please pick a preset:
  Default ([Vue 2] babel, eslint)                  >> 預設是基本開的Babel + ESLint preset
  Default (Vue 3 Preview) ([Vue 3] babel, eslint)  >> 預設是基本開的Babel + ESLint preset
> Manually select features                         >> 自己挑選
```

### 自選建議選項
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

### 安裝版本?
```
? Choose a version of Vue.js that you want to start the project with
  2.x
> 3.x (Preview)
```

### 路由
```
? Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n) Y
```
Y:http://localhost:8080/about

N:http://localhost:8080/#/about

### CSS套件選擇
```
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default):
  Sass/SCSS (with dart-sass)
> Sass/SCSS (with node-sass)
  Less
  Stylus
```

### 規則
```
? Pick a linter / formatter config:
  ESLint with error prevention only
  ESLint + Airbnb config
> ESLint + Standard config
  ESLint + Prettier
```

### 執行時間
```
? Pick additional lint features: (Press <space> to select, <a> to toggle all, <i> to invert selection)
>(*) Lint on save
 ( ) Lint and fix on commit
```

### 設定檔存在哪?
```
? Where do you prefer placing config for Babel, ESLint, etc.?
  In dedicated config files
> In package.json
```

### 是否存為下次專案預設
```
? Save this as a preset for future projects? (y/N) N
```

---

## 環境變數
```javascript
.env                # loaded in all cases
.env.local          # loaded in all cases, ignored by git
.env.[mode]         # only loaded in specified mode
.env.[mode].local   # only loaded in specified mode, ignored by git
```

```javascript
NODE_ENV=development
VUE_APP_SECRET=envdevelopment
```

```javascript
console.log(process.env.VUE_APP_NOT_SECRET_CODE)
```

```javascript
  "scripts": {
    "serve": "vue-cli-service serve",
    "serve-dev": "vue-cli-service serve --mode development",
    "serve-test": "vue-cli-service serve --mode test",
    "serve-pro": "vue-cli-service serve --mode production",
    "build": "vue-cli-service build",
    "build-dev": "vue-cli-service build --mode development",
    "build-test": "vue-cli-service build --mode test",
    "build-pro": "vue-cli-service build --mode production",
    "lint": "vue-cli-service lint"
  },
```

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

## 