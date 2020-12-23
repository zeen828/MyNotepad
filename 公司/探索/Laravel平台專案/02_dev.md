# 公司文件 - 探索 - Laravel開發專案 - 開發紀錄

## 博彩

## id轉換
appid和uid轉換
app\Libraries\Traits\Entity\Swap\Identity.php
asPrimaryTid => 轉換成TID
針對model做處理

```php
use App\Libraries\Traits\Entity\Swap\Identity;
class Demo extends Model implements Transformable
{
    use Identity;
    // Did  大寫這是laravel改寫鉤子
    public function getDidAttribute(): ?string
    {
        // 這是Libraries裡寫的getTidAttribute()鉤子
        return ($this->exists ? $this->tid : null);
    }
}
```
Entities增加宣告

```php
    public function transform(GameDraw $model)
    {
         return collect([
            'id' => (int) $model->id,
            /* place your other model properties here */
            'did' => $model->did,
        ])->map(function ($item, $key) {
            return (isset($item) ? $item : '');
        })->all();
    }
```
App\Transformers\DemoTransformer.php



### 用戶一般會員
設計一個有上下線階層的會員系統

#### 資料庫調整
    php artisan make:migration MembersAppendField

### 測試用
php artisan make:command Tests/TestModelJob
// php artisan tests:model


## 路由權限問題
1. 沒有任何登入都會阻攔

2. 登入Client可以透過中間件做檢查規避，如果是user或其他的事無法規避的
```
    ->middleware('token.login');
```

3. 可以透過設定檔做一些訪問阻攔
confin/ban.php