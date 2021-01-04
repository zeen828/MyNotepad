# 這是我的記事本
一些程式開發面的東西做一些小記錄

## 目錄
<details>
<summary>展开查看</summary>
<pre><code>
├── 公司
│   └── 探索
│       ├── 代購網站
│       │   └── 01_Start.md
│       ├── 代購網站
│       │   └── 01_Start.md
│       ├── 愛碼彩票
│       │   └── 01_Start.md
│       ├── 購物車客戶
│       │   └── 01_Start.md
│       ├── jack娛樂
│       │   └── 01_Start.md
│       ├── Laravel平台專案
│       │   ├── 01_Start.md
│       │   └── 02_dev.md
│       └── Sharksphim
│           └── 01_Start.md
├── CentOS
│   └── Nginx
│       └── 01.conf.md
├── Git
│   └── 01_Instruction.md
├── Golang
│   └── 01_Start.md
├── Laravel
│   ├── 01_Start.md
│   ├── 02_Laravel_Admin.md
│   ├── 03
│   ├── 04
│   └── 05_L5_Repositories.md
├── VueJS
│   └── 01_Start.md
├── XAMPP
│   └── 01_Start.md
└── README.md
</code></pre>
</details>


## 物件宣告
static(靜態變數)：使用時不需要特別建立物件，就可以直接使用；例如：類別名稱::$static變數;

public(公有變數)：必須建立物件後才可以使用，但是可以在類別以外的地方做使用；例如：$變數 = new 類別();  $變數->public變數;(不需加$字號)

protected：必須建立物件後才可以使用，不可以在類別以外的地方做使用，但是可以被繼承並在子類別使用，範例如下

private(私有變數)：必須建立物件後才可以使用，只可以在這個類別內使用且不能被繼承

## 物件內部呼叫
```php
$this->demo();
self::demo();
```

## 物件內建方法
```php
    public function __construct()
    {
        parent::__construct();
    }
    public function __destruct()
    {
        parent::__destruct();
    }
```

```
__call($name, $arguments)	當呼叫不存在的方法時執行
__callStatic($name, $arguments)	當呼叫不存在的 static 方法時執行
__get($name)	當讀取不存在的屬性時執行
__set($name, $value)	當設定不存在的屬性時執行
__isset($name)	當使用不存在的屬性呼叫 isset() 或 empty() 時執行
__unset($name)	當使用不存在的屬性呼叫 unset() 時執行
__sleep()	在物件序列化之前執行
__wakeup()	在物件序列化過程中重建物件
__toString()	物件的字串形式
__invoke($x)	當把物件當函數呼叫時執行
__set_state($an_array)	當呼叫 var_export() 方法時執行
__clone()	使用 clone 複製物件完成後執行
```

## 術語
```
類別	class
子類別	subclass
父類別	superclass
物件	object
物件導向程式設計	object-oriented programming
介面	interface
特徵機制	trait
迭代器	iterator
建構子	constructor
解構子	destructor
關鍵字	keyword
識別字	identifier
存取標籤	access label
繼承	inheritance
迴圈	loop
屬性	property
陣列	array
方法	method
別名	alias
參數列	parameter list
成員	member
常數	constant
變數	variable
函數	function
封裝	encapsulation
```