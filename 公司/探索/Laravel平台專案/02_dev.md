# 公司文件 - 探索 - Laravel開發專案 - 開發紀錄

> ## `id編碼轉換`
> appid和uid轉換
> 
> 函式庫在：app\Libraries\Traits\Entity\Swap\Identity.php
> 
> asPrimaryTid => 轉換成TID
> 
> asPrimaryId => 轉換成ID
> 
> 針對model做處理
> 
> ### `id轉碼`
> ```php
> namespace App\Entities;
> // 添加
> use App\Libraries\Traits\Entity\Swap\Identity;
> class Demo extends Model implements Transformable
> {
>     // 添加
>     use Identity;
>     // 添加laravel改寫鉤子 (Did大寫)
>     public function getDidAttribute(): ?string
>     {
>         // 這是Libraries裡寫的getTidAttribute()鉤子
>         return ($this->exists ? $this->tid : null);
>     }
> }
> ```
> Entities增加宣告
> 
> ```php
> namespace App\Transformers;
> class DemoTransformer extends TransformerAbstract
> {
>     public function transform(GameDraw $model)
>     {
>          return collect([
>             'id' => (int) $model->id,
>             // 添加要對應的顯示
>             'did' => $model->did,
>         ])->map(function ($item, $key) {
>             return (isset($item) ? $item : '');
>         })->all();
>     }
> }
> ```
> Transformer配置使用名稱
> 
> 範例 => id轉成did
> 
> ### `解碼`
> ```php
> // Model要存在才有辦法去轉換
> use App\Entities\Demo;
>         $mid = '194585914';
>         // 調用mopdel
>         $id = app(Demo::class)->asPrimaryId($mid);
>         var_dump($id);
> ```
> 轉換回正常id

---

### 用戶一般會員
設計一個有上下線階層的會員系統

#### 資料庫調整
    php artisan make:migration MembersAppendField

### 測試用
php artisan make:command Tests/TestModelJob
// php artisan tests:model

---

## 路由權限問題
1. 沒有任何登入都會阻攔

2. 登入Client可以透過中間件做檢查規避，如果是user或其他的事無法規避的
```
    ->middleware('token.login');
```

3. 可以透過設定檔做一些訪問阻攔
confin/ban.php


## 貨幣
trade_log
D:\taki\bet_api\app\Libraries\Traits\Entity\Trade\Ables.rd.php

## 單據
receipts

## 合併程式注意
1. token要對
2. token.login要移除