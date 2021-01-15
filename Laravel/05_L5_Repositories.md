# Laravel - L5 Repositories
Laravel抽象結構

---

## 安裝套件
>     composer require prettus/l5-repository
> 
> ```
> 'providers' => [
>     ...
>     Prettus\Repository\Providers\RepositoryServiceProvider::class,
> ],
> ```
> `config/app.php`添加設定
> 
> ```
> php artisan vendor:publish --provider "Prettus\Repository\Providers\RepositoryServiceProvider"
> ```
> 發布設定，會產生`/config/repository.php`
> 
> [參考](https://packagist.org/packages/prettus/l5-repository)

---

## 抽象結構
> - MVC
>     - Model (模型)
>     - Controller (控制器)
>     - View (示圖)
> - L5-R (Repository-Service-Presenter)
>     - Entity (模型)
>     - Repository (模型倉庫)
>         - Criteria (標準條件)
>         - Validator (驗證器)
>         - Presenter (呈現器)
>             - Transform (轉換器)
>     - Controller (控制器)
>         - Request (輸入條件)
>         - Response ()
> ```
> Model ---> Repository ---> Service ---> Controller ---> Presenter ---> View
> ``` 
> ```
> The Controller     # 控制器
> The Validator      # 请求验证
> The Model          # Model
> The Repository     # 数据库模型
> The Presenter      # 不是必须的
> The Transformer    # 不是必须的
> ```

---

## 快速紀錄
### Criteria 標準條件
```
// 預設標準
vendor\prettus\l5-repository\src\Prettus\Repository\Criteria\RequestCriteria.php

```

```
php artisan make:criteria Demo/Index
```
建立標準條件

```php
    public function apply($model, RepositoryInterface $repository)
    {
        // return $model;
        $query = $model->where('status', '=', '1');
        return $query;
    }
```
app\Criteria\Demo\IndexCriteria.php

```php
use App\Criteria\Demo\IndexCriteria;
        // 增加標準查詢
        $this->repository->pushCriteria(app('App\Criteria\Demo\IndexCriteria'));
        $demo = $this->repository->all();
        // 忽略標準查詢
        $demo = $this->repository->skipCriteria()->all();
// OR
        // 可以取代pushCriteria()->all()
        $demo = $this->repository->getByCriteria(new IndexCriteria());
```
一些使用方式

### Validator 資料庫操作基本規則
```php
use App\Validators\DemoValidator;
    public function validator()
    {

        return DemoValidator::class;
    }
```
app\Repositories\DemoRepositoryEloquent.php

```php
    protected $rules = [
        ValidatorInterface::RULE_CREATE => [
            'title' => 'required',
        ],
        ValidatorInterface::RULE_UPDATE => [],
    ];

```
app\Validators\DemoValidator.php

### Presenters 顯示格式化
```php
    public function presenter()
    {
        return "App\Presenters\DemoPresenter";
    }
// OR
$this->repository->setPresenter("App\Presenters\DemoPresenter");
```
app\Repositories\DemoRepositoryEloquent.php

增加設定

```php
use App\Transformers\DemoTransformer;
    public function getTransformer()
    {
        return new DemoTransformer();
    }
```
app\Presenters\DemoPresenter.php

路過沒用到的檔案

```php
    public function transform(GameSetting $model)
    {
         return collect([
            'id' => (int) $model->id,
            /* place your other model properties here */
            'name' => $model->name,
            'status' => $model->status,
            /* Timezone datetime */
            'created_at' => $model->created_at,
            'updated_at' => $model->updated_at
        ])->map(function ($item, $key) {
            return (isset($item) ? $item : '');
        })->all();
    }
```
app\Transformers\DemoTransformer.php

設定顯示的資料處理

```php
    public function transform(array &$data)
    {
        // Adjust the response format data label
        $named = request()->route()->getName();
        switch ($named) {
            case 'demo.read':
                // 單筆資料不顯示的欄位
                $data = collect($data)->forget([
                    'id',
                    'password',
                    'status'
                ])->all();
                break;
            case 'demo.list':
                // 多筆資料不顯示的欄位
                $data['data'] = collect($data['data'])->map(function ($info) {
                    return collect($info)->forget([
                        'id',
                        'password',
                        'status',
                    ])->all();
                })->all();
                break;
        }
    }
```
app\Http\Responses\DemoCreateResponse.php

設定不顯示的欄位

```php
$repository = app('App\Repositories\DemoRepository');
$repository->setPresenter("Aapp\Presenters\DemoPresenter");

//Getting the result transformed by the presenter directly in the search
$demo = $repository->find(1);
print_r($demo); //It produces an output as array

//Skip presenter and bringing the original result of the Model
$demo = $repository->skipPresenter()->find(1);
print_r($demo); //It produces an output as a Model object
print_r($demo->presenter()); //It produces an output as array
```
使用範例


---

## 預設提供方法
```php
    // 從倉庫中取得全部數據
    $posts = $this->repository->all();
    // 從倉庫中取得分頁數據
    $posts = $this->repository->paginate($limit = null, $columns = ['*']);
    // 從倉庫中取得指定id資料
    $post = $this->repository->find($id);
    // 隱藏模型特定屬性
    $post = $this->repository->hidden(['country_id'])->find($id);
    // 僅顯示模型特定屬性
    $post = $this->repository->visible(['id', 'state_id'])->find($id);
    // 加載模型關聯關係
    $post = $this->repository->with(['state'])->find($id);
    // 通過字段值匹配獲取指定資料
    $posts = $this->repository->findByField('country_id','15');
    // 根據多個字段值匹配獲取指定資料
    $posts = $this->repository->findWhere([
        // Default Condition =
        'state_id'=>'10',
        'country_id'=>'15',
        // Custom Condition
        ['columnName','>','10']
    ]);
    // 根據字段是否存在多個植獲取資料
    $posts = $this->repository->findWhereIn('id', [1,2,3,4,5]);
    // 根據字段是否布存在多個植獲取資料
    $posts = $this->repository->findWhereNotIn('id', [6,7,8,9,10]);
    // 使用者自訂查詢範圍取得全部數據
    $posts = $this->repository->scopeQuery(function($query){
        return $query->orderBy('sort_order','asc');
    })->all();
    // 在倉庫中創建新的紀錄
    $post = $this->repository->create( Input::all() );
    // 在倉庫中更新紀錄
    $post = $this->repository->update( Input::all(), $id );
    // 在倉庫中刪除紀錄
    $this->repository->delete($id)
    // 在倉庫中刪除紀錄
    $this->repository->deleteWhere([
        //Default Condition =
        'state_id'=>'10',
        'country_id'=>'15',
    ])
    // 加入標準
    $this->repository->pushCriteria(new MyCriteria());
    // 移除標準
    $this->repository->popCriteria(new MyCriteria());
    // 根標準條件獲取結果資料
    $posts = $this->repository->getByCriteria(new MyCriteria());
    // 跳過倉庫中定義的標準條件取得全部數據
    $posts = $this->repository->skipCriteria()->all();

測試用
php artisan make:criteria LotteryGames/GameSetting/Admin
php artisan make:criteria LotteryGames/GameSetting/User
php artisan make:criteria LotteryGames/GameRuleType/Admin
php artisan make:criteria LotteryGames/GameRuleType/User
php artisan make:criteria LotteryGames/GameRule/Admin
php artisan make:criteria LotteryGames/GameRule/User
php artisan make:criteria LotteryGames/GameDraw/Admin
php artisan make:criteria LotteryGames/GameDraw/User
php artisan make:criteria LotteryGames/GameDraw/QueryHistory
php artisan make:criteria LotteryGames/GameDraw/JobGameReward
//php artisan make:criteria LotteryGames/GameDraw/BetDelay
php artisan make:criteria LotteryGames/GameBet/Admin
php artisan make:criteria LotteryGames/GameBet/User
php artisan make:criteria LotteryGames/GameBet/QueryRecord


php artisan make:criteria Member/User/Admin


php artisan make:entity LotteryGames/GameSetting
php artisan make:entity LotteryGames/GameRule
php artisan make:entity LotteryGames/GameRuleType
php artisan make:entity LotteryGames/GameDraw
php artisan make:entity LotteryGames/GameBet
```

---

## 建立Model
>     php artisan make:entity Members
> 透過L5-R工具指令建立Model
> 
> ```
> D:\taki\Project>php artisan make:entity members
>  Would you like to create a Presenter? [y|N] (yes/no) [no]:
>  > yes
> App\Transformers\MembersTransformerPresenter created successfully.
>  Would you like to create a Transformer? [y|N] (yes/no) [no]:
>  > yes
> Transformer created successfully.
>  Would you like to create a Validator? [y|N] (yes/no) [no]:
>  > yes
> Validator created successfully.
>  Would you like to create a Controller? [y|N] (yes/no) [no]:
>  > yes
> 
> Request created successfully.
> Request created successfully.
> Controller created successfully.
> Repository created successfully.
> Provider created successfully.
> Bindings created successfully.
> ```
> 在建立過程中，會自動詢問是否要同步建立 Presenter，Transformer，Validator， Controller
> 
> ```
> app/Entities/
> app/Http/Controllers/MembersController.php
> app/Http/Requests/
> app/Presenters/
> app/Providers/RepositoryServiceProvider.php
> app/Repositories/
> app/Transformers/
> app/Validators/
> database/migrations/2020_11_19_032453_create_members_table.php
> ```
> 建立完總共會有以上檔案
> 

---

## Repositories

### 緩存
```php
use Prettus\Repository\Eloquent\BaseRepository;
use Prettus\Repository\Contracts\CacheableInterface;
use Prettus\Repository\Traits\CacheableRepository;

class MyRepository extends BaseRepository implements CacheableInterface {

    use CacheableRepository;

    ...
}
```

```php
    'cache'=>[
        //Enable or disable cache repositories 是否使用
        'enabled'   => true,
    ]
```
開啟緩存設定config\repository.php



### 格式化
預設沒有啟用需要自行添加
```php
    /**
     * Specify Presenter class name
     *
     * @return string
     */
    public function presenter()
    {
        return "App\\Presenters\\MyPresenter";
    }
```
PS：使用model的操作是不會套用格式的



### Repositories使用model
```php
        $data = $settingRepository->model()::find(1);
        print_r($data);
        $data = $drawRepository->model()::find(1);
        print_r($data);
        $data = $this->setting->model()::find(1);
        print_r($data);
        $data = $this->draw->model()::find(1);
        print_r($data);
```

---

### 參考
[l5-repository 上手 « 關於網路那些事...](https://adon988.logdown.com/posts/7811868-l5-repository)

[l5-repository的使用 - 台部落](https://www.twblogs.net/a/5bbd67732b71776bd30c48be)

[l5-repository 组件 - 简书](https://www.jianshu.com/p/250c7833d2a6)