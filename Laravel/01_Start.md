# Laravel

## 凡事都要一個開頭

---

### 建立一個`Laravel`新專案
有幾種建立新專案的方法舉例兩種利用`Composer`的作法

> #### 透過Composer`全域資源安裝`
>     composer global require laravel/installer
> 透過Composer去將Laravel安裝到全域資源
> 
>     laravel new Project
> 利用`全域資源`的Laravel建立新專案
> 
> `Project`：專案目錄名稱
> 

> #### 透過Composer`直接安裝`
>     composer create-project --prefer-dist laravel/laravel Project
> 透過Composer直接建立新專案
> 
> `Project`：專案目錄名稱

---

### 執行Migrate
    php artisan migrate

---

### 查詢`Laravel`路由
    php artisan route:list

---

### 運行`Laravel`專案
    php artisan serve
運行專案
