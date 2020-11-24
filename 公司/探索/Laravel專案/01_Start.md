# 公司文件 - 探索 - Laravel開發專案

## 安裝
>     git pull
>     composer install
> 需要安裝`Redis`

## Client
> ### 資料庫資料
> > 資料庫有做加密處理無法直接在DB看到資料
> > 
> > DB資料操作須在指令下達
> > 
> >     php artisan data:client-add
> > 新增Client
> > 
> >     php artisan data:client-read
> > 讀取Client
> > 
> >     php artisan data:client-edit
> > 編輯Client

### 預設資料
>     1294583

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

## 查詢
>     /api/v1/auth/client (Clinet Index)
> 查詢所有client Service