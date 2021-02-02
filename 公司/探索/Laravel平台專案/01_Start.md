# 公司文件 - 探索 - Laravel開發專案

## 版本
Laravel 7.22

PHP 7.4

---

## 安裝
> ```
> git pull
> composer install
> //需要安裝`Redis`
> php artisan migrate
> php artisan db:seed
> php artisan data:client-read
> //1294583
> php artisan data:client-add
> // Admin Service, 1, yes
> php artisan data:client-add
> // Member Service, 2, yes
> php artisan data:client-add
> // Server Service, 3, yes
> php artisan data:client-add
> // Business Service, 4, yes
> ```

---

## 文件
```
php artisan apidoc:generate
```
[參考](https://www.vnewin.com/day29-laravel-automatically-generates-api-files/)

---

## Client
>> ### 資料庫資料
>> 資料庫有做加密處理無法直接在DB看到資料
>> 
>> DB資料操作須在指令下達
>> 
>>     php artisan data:client-add
>> 新增Client
>> 
>>     php artisan data:client-read
>> 讀取Client
>> 
>>     php artisan data:client-edit
>> 編輯Client
> 
> ### 預設資料
>     1294583

---

## 程式結構
    app\Console\Commands\Data\Read\JwtClient.php
自訂 `php artisan data:client-read` 程式位置

    app\Libraries\Traits\Entity\Swap\Identity.php
`asPrimaryId` 解密位置

    app\Transformers\Jwt\ClientTransformer.php
回傳結構(format data)

---

## 指令
>> #### make
>>     make:currency
>>     php artisan make:currency demo_currency
>> Create a new Eloquent model account `Currency(貨幣)` class
>> 
>>     make:ex-code
>>     php artisan make:ex-code demo_ex_code
>> Create a new `ExceptionCode(異常代碼)` class
>> 
>>     make:ex-converter
>>     php artisan make:ex-converter demo_ex_converter
>> Create a new `exception converter language(異常語系)` file
>> 
>>     make:feature
>>     php artisan make:feature demo_feature
>> Create a new `Feature(特徵)` class
>> 
>>     make:feature-document
>>     php artisan make:feature-document demo_feature_document
>> Create a `feature document language(特徵語系)` file
>> 
>>     make:notification-sms
>>     php artisan make:notification-sms demo_notification_sms
>> Create a new `SMS notification(電信SMS)` class for telecomer
>> 
>>     make:response
>>     php artisan make:response demo_response
>> Create a new `response success conversion` class
>> 
>>     make:sp-document
>>     php artisan make:sp-document demo_sp_document
>> Create a `system parameter document language(系統參數語系)` file
>> 
>>     make:user-auth
>>     php artisan make:user-auth demo_user_auth
>> Create a new `Eloquent model user Auth` class
>> 
>>     make:user-observer
>>     php artisan make:user-observer demo_user_observer
>> Create a new `Eloquent model user Observer(觀察者)` class
> 
>> #### mg-column
>>     mg-column:append-authority
>>     php artisan mg-column:append-authority demo_will_tests
>> Append a `authority(API 訪問權配置內容)` column for database table(更新舊表格)
>> 
>>     mg-column:append-feature
>>     php artisan mg-column:append-feature demo_will_tests
>> Append a `feature(特徵配置內容)` column for database table(更新舊表格)
>> 
>>     mg-column:append-setting
>>     php artisan mg-column:append-setting demo_will_tests
>> Append a `setting(附屬配置內容)` column for database table(更新舊表格)
>> 
>>     mg-column:append-unique-auth
>>     php artisan mg-column:append-unique-auth demo_will_tests
>> Append a `unique_auth(唯一身份授權碼)` column for database table(更新舊表格)
> 
>> #### mg-table
>>     mg-table:create-currency
>>     php artisan mg-table:create-currency demo_will_tests_currency
>> Create a `currency account table(貨幣帳戶表)` for database(建立新表格)
> 
>> #### service
>>     service:add-auth-observer
>>     php artisan service:add-auth-observer demo_auth_observer
>> Insert auth guard observer basic configuration class
>> 
>> 寫入app\Providers\AppServiceProvider.php
> 
>> #### trade
>>     trade:pause
>>     php artisan trade:pause 28
>> Suspend trading services and automatically activate after a specified number of minutes.
>> 
>> 暫停交易服務，並在指定的分鐘數後自動激活。

---

## 流程
>     /api/v1/auth/token (Auth / Get Token)
> Client資訊`client_id` & Client資訊`client_secret` 取得 `Client的access_token`
>
>     /api/v1/auth/user/login/admin (Auth / User Login)
> `Client的access_token` & User資訊`account` & User資訊`password` 取得 `User的access_token`
> 
>>     /api/v1/auth/user/signature (Auth / User Signature)
>> `User的access_token` 取得 User的`signature`
> 
>     /api/v1/auth/user/signature/login (Auth / User Signature Login)
> `Client的access_token` & User的`signature` 取得 `User的access_token`
> 
>     /api/v1/system/interface (System / Interface / APIs)
> `Client的access_token` | `User的access_token` 取得 可配置權限
> 
> /api/v1/system/interface/managed

---

## 查詢
>     /api/v1/auth/client (Clinet Index)
> 查詢所有client Service

---

## 開發流程
> ### 範例
>> 1. 建立整套
>> ```
>>     php artisan make:entity Demo/WillTest
>> ```
> 
>> 2. 開啟`Requests`下的授權
>> ```
>>     public function authorize()
>>     {
>>         // return false;
>>         return true;
>>     }
>> ```
>> app\Http\Requests\Demo\WillTestCreateRequest.php
>> 
>> app\Http\Requests\Demo\WillTestUpdateRequest.php
> 
>> 3. 配置資料表欄位
>> ```
>> 	public function up()
>> 	{
>> 		Schema::create('demo_will_tests', function(Blueprint $table) {
>>             $table->increments('id');
>> 			$table->string('account')->unique()->comment('帳號');
>> 			$table->tinyInteger('status')->unsigned()->default(1)->comment('狀態0:停權1:授權');
>>             $table->timestamps();
>> 		});
>> 	}
>> ```
>> database\migrations\2020_11_25_074314_create_demo_will_tests_table.php
>> 
>> 調整資料表欄位
> 
>> 4. 設定model
>> ```
>>     protected $table = 'demo_will_tests';
>> 
>>     protected $fillable = [
>>         'account',
>>         'status',
>>     ];
>> ```
>> app\Entities\Demo\WillTest.php
> 
>> 5. 配置路由
>> ```
>> Route::resource('demo/willtest', 'Demo\WillTestController');
>> ```
>> routes\api.php
> 
>> 6. 測試
>> 資料庫建立假資料
>> ```
>> GET http://127.0.0.1:8000/api/demo/willtest
>> ```
>> Postman測試該路徑


## 建立一個Admin User API順序
> ### 最大權限管理者
> 1. Auth > Get Token（取Client access_token）
> 
>        /api/v1/auth/token
> 
> 2. Auth > User Login（取Admin User access_token）
> 
>        /api/v1/auth/user/login/admin
> 
> 3. Admin > User > Admin Logon（建立/登入Admin）
> 
>        /api/v1/admin/user/logon
> 
> 4. Admin > User > User Index（查Admin User清單）
> 
>        /api/v1/admin/user?page=1&row=15&start=2020-11-01&end=2021-12-30
> 
> 5. System > Authority > Interface > APIs（查權限清單）
> 
>        /api/v1/system/interface
> 
> 6. System > Authority > Grant Authority（發派Admin User權限）
> 
>        /api/v1/system/authority/grant/admin/3515611
> 
> 7. Admin > User > Resend Auth（重發Admin User認證信）
> 
>        /api/v1/admin/user/resend/auth/1294583
 
> ### 新帳號
> 1. Auth > Get Token（取Client access_token）
> 
>        /api/v1/auth/token
> 
> 2. Auth > User Signature Login（驗證信箱檢查碼）
> 
>        /api/v1/auth/user/signature/login
> 
> 3. Admin > Auth > Change Password（第一次設定密碼/修改密碼）
> 
>        /api/v1/admin/auth/password
> 
> 4. Auth > Get Token（取Client access_token）
> 
>        /api/v1/auth/token
> 
> 5. Auth > Get Token（取Client access_token）
> 
>        /api/v1/auth/token
> 
> 6. Auth > User Login（取Admin User access_token）
> 
>        /api/v1/auth/user/login/admin
> 
> 7. Admin > Auth > Admin Read Me（Admin User個人資訊）
> 
>        /api/v1/admin/auth/me
> 
> 8. Admin > Auth > Edit Profile（Admin User編輯個人資料）
> 
>        /api/v1/admin/auth/profile




# 開發
1. 路由
2. 模型(Model)
   1. (Entity)
   2. 倉庫(Repository)
   3. 123
3. 控制器(Controller)
4. 示圖(View)



> - MVC
>     - Model (模型)
>     - View (示圖)
>     - Controller (控制器)
> - L5-R (Repository-Service-Presenter)
>     - Entity ()
>     - Repository (倉庫)
>         - Criteria ()
>     - Presenter (呈現器)
>     - Transform (轉換器)
>     - Validator (驗證器)
Repository Interface
Repository Criteria Interface
Cacheable Interface
Presenter Interface
Presentable
Criteria Interface
Transformable