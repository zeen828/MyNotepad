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

---

## 抽象結構
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
> 
> ```
> Model ---> Repository ---> Service ---> Controller ---> Presenter ---> View
> ```
> 

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

### 參考
[l5-repository 上手 « 關於網路那些事...](https://adon988.logdown.com/posts/7811868-l5-repository)

[l5-repository的使用 - 台部落](https://www.twblogs.net/a/5bbd67732b71776bd30c48be)

[l5-repository 组件 - 简书](https://www.jianshu.com/p/250c7833d2a6)