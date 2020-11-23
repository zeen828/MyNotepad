# Laravel-開始

## 凡事都要一個開頭

---

## 建立一個`Laravel`新專案
有幾種建立新專案的方法舉例兩種利用`Composer`的作法

> ### 透過Composer`全域資源安裝`
>     composer global require laravel/installer
> 透過Composer去將Laravel安裝到全域資源
> 
>     laravel new Project
> 利用`全域資源`的Laravel建立新專案
> 
> `Project`：專案目錄名稱
> 

> ### 透過Composer`直接安裝`
>     composer create-project --prefer-dist laravel/laravel Project
> 透過Composer直接建立新專案
> 
> `Project`：專案目錄名稱

> ### 透過Composer`直接安裝`指定版本
>     composer create-project --prefer-dist laravel/laravel Project 7.22.*
> 
> `Project`：專案目錄名稱

---

## Laravel `Controller`
建立

---

## Laravel `Model`
>     php artisan make:model Model/ModelName
> 建立

---

## Laravel `Migrate(遷移)`
>     php artisan migrate
> 執行Laravel預設Migrate
> 
>     php artisan make:migration MigrateNme --path="database/migrations/20200101
> 建立Migrate檔案
> 
> `--path=`：指定路徑，需要自行先創立目錄，`系統不會幫忙創`
> 
>     php artisan migrate --path="database/migrations/20200818_user/"
> 執行指定目錄下的Migrate
> 
> `--path=`：指定路徑
> 
>     php artisan migrate:rollback --path="database/migrations/20200818_user"
> 回復指定路徑下的Migrate，回復預設還是以Migrate Tabale順序又大開始返回

---

## Laravel `Seed`
>     php artisan db:seed
> 執行Laravel預設Seed
> 
>     php artisan make:seeder ClassNameSeeder
> 建立Seed檔案
> 
>     php artisan db:seed --ClassNameSeeder
> 執行指定Seed檔案

---

## Laravel `Route(路由)`
>     php artisan route:list
> 查看目前所有路由
---

## Laravel `運行`
>     php artisan serve
> 運行專案

## Laravel `清除緩存`
>     composer clear-cache
>     composer dump-autoload
>     php artisan cache:clear
>     php artisan view:clear
>     php artisan config:cache
>     php artisan route:clear
