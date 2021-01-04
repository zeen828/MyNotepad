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

## Laravel `產生key`
>     php artisan key:generate
> .env產生一組新的KEY

## Laravel `Controller`
> 建立Controller

---

## Laravel `Model`
>     php artisan make:model Model/ModelName
> 建立Model

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
>     php artisan config:clear
>     php artisan route:clear
>
>     php artisan cache:cache
>     php artisan view:cache
>     php artisan config:cache
>     php artisan route:cache

## mailtrap代理信箱設定
```php
MAIL_DRIVER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=25
MAIL_USERNAME=571853ad511c2f
MAIL_PASSWORD=bca595bbab81c1
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS=from@example.com
MAIL_FROM_NAME=Example
```

```
// Inboxes > list action > 設定 > SMTP Settings
curl --ssl-reqd \
--url 'smtp://smtp.mailtrap.io:2525' \
--user '571853ad511c2f:bca595bbab81c1' \
--mail-from from@example.com \
--mail-rcpt to@example.com \
```

## 資料庫臨時關閉嚴格模式
如果使用GROUP BY卻沒在SELECT有該欄位會因為嚴格模式報錯
```
Syntax error or access violation: 1055
```
```PHP
// 臨時關閉
\DB::statement("SET SQL_MODE=''");
```
