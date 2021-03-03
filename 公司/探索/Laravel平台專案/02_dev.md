# 公司文件 - 探索 - Laravel開發專案 - 開發紀錄



---

### 用戶一般會員
設計一個有上下線階層的會員系統

php artisan make:user-auth Backstage

#### 資料庫調整
    php artisan make:migration MembersAppendField

---

## 路由權限問題
1. 沒有任何登入都會阻攔

2. 登入Client可以透過中間件做檢查規避，如果是user或其他的事無法規避的
```
    ->middleware('token.login');
```

3. 可以透過設定檔做一些訪問阻攔
confin/ban.php




### 會員資料
```php
use App\Entities\Account\Gift;// 贈點(自訂幣)
use App\Entities\Account\Gold;// 金幣
use App\Entities\Jwt\Auth;// Clinet
use App\Entities\Member\Auth as MemberAuth;// Member User
use TokenAuth;
use DB;

    // 交易對象
    $client = TokenAuth::getClient();
    $user = TokenAuth::getUser();

    // 錢包生成
    $giftAccount = $user->tradeAccount(Gift::class);// 贈點錢包
    $goldAccount = $user->tradeAccount(Gold::class);// 金幣錢包

    // 交易單據
    $item = $giftAccount->where('id', $giftAccount->id)->lockForUpdate()->first();
    $trade = $item->beginTradeAmount($client, $order);
    $order = $trade->amountOfExpenses($amount, [
        'label' => 'PLAY_GAME_BET',
        'content' => 'EXPENSES',
        'note' => array(
            'betId' => $betId,
            'sum' => $amount
        )
    ]);
```

### 交易ID
```php
    $accountId = $user->trade_account_id;
```

### 貨幣
```php
use App\Entities\Member\Auth as MemberAuth;
    $item = app(MemberAuth::class);
    $currencys = $item->heldCurrencyModels();
    var_dump($currencys);

    $item = app(MemberAuth::class);
    $columns = [
        'class',
        'type',
        'description'
    ];
    $currencys = $item->heldCurrencyTypes($columns);
    var_dump($currencys);
```

### 查貨幣
```php
use App\Entities\Account\Gold;
use DB;
    $accountId = $user->trade_account_id;
    // DB Transaction begin
    DB::beginTransaction();
    $account = Gold::where('id', $accountId)->sharedLock()->first();
    // Get the balance amount
    $amount = $account->amount;
    DB::commit();
    var_dump($account);
    var_dump($amount);
```

### 添加貨幣
```php
use App\Entities\Account\Gold;
use App\Entities\Jwt\Auth;
use App\Entities\Member\Auth as MemberAuth;
use DB;
    $account = $user->tradeAccount(Gold::class);
    $target = Auth::find(1);
    // DB Transaction begin
    DB::beginTransaction();
    $item = $account->where('id', $account->id)->lockForUpdate()->first();
    $trade = $item->beginTradeAmount($target);
    // Amount of income by trade
    $order = $trade->amountOfIncome(35, [
        'content' => 'INCOME',
        'note' => [
            'Custom notes 1'
        ]
    ]);
    DB::commit();
```

### 消耗貨幣
```php
            // $accountId = $user->trade_account_id;
            // DB::beginTransaction();

            // $account = Gold::where('id', $accountId)->lockForUpdate()->first();
            //var_dump($account);
        
            // $target = Auth::find(2);
            // // Create trade
            // $trade = $account->beginTradeAmount($target);
            // // Amount of expenses by trade
            // $order = $trade->amountOfExpenses(500, [
            //            'label' => 'member-trade',
            //            'content' => 'EXPENSES',
            //            'note' => 'Custom notes 1'
            //         ]);
            // // Amount of expenses by trade
            // $order2 = $trade->amountOfExpenses(10, [
            //            'label' => 'fee',
            //            'content' => 'EXPENSES',
            //            'note' => 'Custom notes 2'
            //         ]);
            // // Get the balance amount
            // $account->amount;
            // // Get order number
            // $order->order;
            // // Get serial number
            // $order->serial;
            // // Get order number = $order->order
            // $order2->order;
            // // Get serial number
            // $order2->serial;
        
            // DB::commit();

```

#### Auth()
```php
use App\Entities\Account\Gold;
use App\Entities\Account\Gift;
use App\Entities\Jwt\Auth;
use App\Entities\Member\Auth as MemberAuth;
use DB;
    // Auth Usage 3 :
    $user = MemberAuth::find(1);
    $accountId = $user->trade_account_id;
    var_dump($accountId);
    // int(102)

    // Auth Usage 4 :
    $item = app(MemberAuth::class);
    $currencys = $item->heldCurrencyModels();
    var_dump($currencys);
    // array(1) {
    //     ["gold"]=>
    //     string(25) "App\Entities\Account\Gold"
    // }

    // Auth Usage 5 :
    $item = app(MemberAuth::class);
    $columns = [
        'class',
        'type',
        'description'
    ];
    $currencys = $item->heldCurrencyTypes($columns);
    var_dump($currencys);
    // array(1) {
    //     ["gold"]=>
    //     array(3) {
    //         ["class"]=>
    //         string(25) "App\Entities\Account\Gold"
    //         ["type"]=>
    //         string(4) "gold"
    //         ["description"]=>
    //         string(4) "gold"
    //     }
    // }

    // Auth Usage 6 :
    $user = MemberAuth::find(1);
    $account = $user->tradeAccount(Gold::class);
    $target = Auth::find(1);
    // DB Transaction begin
    DB::beginTransaction();
    $item = $account->where('id', $account->id)->lockForUpdate()->first();
    $trade = $item->beginTradeAmount($target);
    // Amount of income by trade
    $order = $trade->amountOfIncome(100, [
        'content' => 'INCOME',
        'note' => [
            'Custom notes 1'
        ]
    ]);
    DB::commit();
    var_dump($order);
```

#### Ables()
```php
use App\Entities\Jwt\Auth;
use App\Entities\Account\Gold;
use App\Entities\Account\Gift;
    // Ables Step 1

    // Ables Step 2
    $item = app(Auth::class);
    $accountables = $item->getTradeAccountables();
    var_dump($accountables);
    // array(1) {
    //     ["gold"]=>
    //     string(25) "App\Entities\Account\Gold"
    // }

    // Ables Step 3
    $item = app(Auth::class);
    $type = $item->getTradeAccountableType(Gold::class);
    var_dump($type);
    // string(4) "gold"

    // Ables Step 4
    $item = app(Auth::class);
    $type = $item->getTradeAccountableType(Gold::class);
    $model = $item->getTradeAccountableModel($type);
    var_dump($model);
    // string(25) "App\Entities\Account\Gold"

    // Ables Step 5
    $item = app(Auth::class);
    $code = $item->getTradeAccountableCode(Gold::class);
    var_dump($code);
    // int(1)

    // Ables Step 6
    $item = app(Auth::class);
    $holderModles = $item->takeTradeAccountableHolders(Gold::class);
    var_dump($holderModles);
    // array(1) {
    //     [2]=>
    //     string(24) "App\Entities\Member\Auth"
    // }

    // Ables Step 7
    $item = app(Auth::class);
    $status = $item->isTradeAccountableAllowed(Gold::class);
    var_dump($status);
    // bool(true)

    // Ables Step 8
    $item = app(Auth::class);
    $sourceables = $item->getTradeSourceables();
    var_dump($sourceables);
    // array(3) {
    //     ["system"]=>
    //     string(21) "App\Entities\Jwt\Auth"
    //     ["member"]=>
    //     string(24) "App\Entities\Member\Auth"
    //     ["admin"]=>
    //     string(23) "App\Entities\Admin\Auth"
    // }

    // Ables Step 9
    $item = app(Auth::class);
    $type = $item->getTradeSourceableType(Auth::class);
    var_dump($type);
    // string(6) "system"

    // Ables Step 10
    $item = app(Auth::class);
    $type = $item->getTradeSourceableType(Auth::class);
    $model = $item->getTradeSourceableModel($type);
    var_dump($model);
    // string(21) "App\Entities\Jwt\Auth"

    // Ables Step 11
    $item = app(Auth::class);
    $code = $item->getTradeSourceableCode(Auth::class);
    var_dump($code);
    // int(1)

    // Ables Step 12
    $item = app(Auth::class);
    $status = $item->isTradeSourceableAllowed(Auth::class);
    var_dump($status);
    // bool(true)

    // Ables Step 13
    $item = app(Auth::class);
    $columns = [
        'class',
        'type',
        'description'
    ];
    $currencys = $item->currencyTypes($columns);
    var_dump($currencys);
    // array(1) {
    //     ["gold"]=>
    //     array(3) {
    //         ["class"]=>
    //         string(25) "App\Entities\Account\Gold"
    //         ["type"]=>
    //         string(4) "gold"
    //         ["description"]=>
    //         string(4) "gold"
    //     }
    // }

    // Ables Step 14
    $item = app(Auth::class);
    $decimal = $item->getAmountDecimal();
    var_dump($decimal);
    // int(0)

    // Ables Step 15
    $item = app(Auth::class);
    $max = $item->getSingleMaxAmount();
    var_dump($max);
    // string(10) "2147483647"

    // Ables Step 16
    $item = app(Auth::class);
    $min = $item->getSingleMinAmount();
    var_dump($min);
    // string(1) "1"

    // Ables Step 17
    $item = app(Auth::class);
    $amount = $item->verifyTradeAmount('100.00');
    var_dump($amount);
    // NULL

    // Ables Step 18
    $item = app(Auth::class);
    $amount = $item->amountFormat('100.00'); 
    var_dump($amount);
    // string(3) "100"
```

#### Currency(貨幣)
```php
use App\Entities\Account\Gold;
use App\Entities\Account\Gift;
use App\Entities\Member\Auth;
use DB;
    // Currency Step 2 :
    // DB Transaction begin
    DB::beginTransaction();
    $account = Gold::where('id', app(Gold::class)->asAccountId(Auth::class, 1))->sharedLock()->first();
    // Get the balance amount
    $amount = $account->amount;
    DB::commit();
    var_dump($amount);
    // string(3) "100"

    // Currency Step 3 :
    // DB Transaction begin
    DB::beginTransaction();
    $account = Gold::where('id', app(Gold::class)->asAccountId(Auth::class, 6))->lockForUpdate()->first();
    $target = Auth::find(1);
    // Create trade
    $trade = $account->beginTradeAmount($target);
    // Amount of income by trade
    $order = $trade->amountOfIncome(100000, [
            'label' => 'member-trade',
            'content' => 'INCOME',
            'note' => 'Custom notes 1'
    ]);
    // Amount of income by trade
    $order2 = $trade->amountOfIncome(10, [
            'label' => 'bonus',
            'content' => 'INCOME',
            'note' => 'Custom notes 2'
    ]);
    // Get the balance amount
    $account->amount;
    // Get order number
    $order->order;
    // Get serial number
    $order->serial;
    // Get order number = $order->order
    $order2->order;
    // Get serial number
    $order2->serial;
    DB::commit();

    // Currency Step 4 :
    // DB Transaction begin
    DB::beginTransaction();
    $account = Gold::where('id', app(Gold::class)->asAccountId(Auth::class, 1))->lockForUpdate()->first();
    $target = Auth::find(2);
    // Create trade
    $trade = $account->beginTradeAmount($target);
    // Amount of expenses by trade
    $order = $trade->amountOfExpenses(100, [
            'label' => 'member-trade',
            'content' => 'EXPENSES',
            'note' => 'Custom notes 1'
            ]);
    // Amount of expenses by trade
    $order2 = $trade->amountOfExpenses(10, [
            'label' => 'fee',
            'content' => 'EXPENSES',
            'note' => 'Custom notes 2'
            ]);
    // Get the balance amount
    $account->amount;
    // Get order number
    $order->order;
    // Get serial number
    $order->serial;
    // Get order number = $order->order
    $order2->order;
    // Get serial number
    $order2->serial;
    DB::commit();

    // Currency Step 5 :
    // DB Transaction begin
    DB::beginTransaction();
    $account = Gold::where('id', app(Gold::class)->asAccountId(Auth::class, 1))->lockForUpdate()->first();
    $target = Auth::find(2);
    // Create trade
    $trade = $account->beginTradeAmount($target);
    // Amount of expenses by trade
    $trade->amountOfExpenses(100, [
        'label' => 'member-trade',
        'content' => 'EXPENSES',
        'note' => 'Custom notes 1'
    ]);
    // Amount of expenses by trade
    $trade->amountOfExpenses(10, [
        'label' => 'fee',
        'content' => 'EXPENSES',
        'note' => 'Custom notes 2'
    ]);
    // Transaction notification for orders
    $list = $trade->tradeNotify();
    DB::commit();
    var_dump($list);
    // bool(true)


    // Currency Step 6 :
    // DB Transaction begin
    DB::beginTransaction();
    $gold = app(Gold::class);
    $accountIds = [];
    // Payer account
    $accountIds[] = $gold->asAccountId(Auth::class, 1);
    // Payee account
    $accountIds[] = $gold->asAccountId(Auth::class, 2);
    $accounts = $gold->whereIn('id', $accountIds)->lockForUpdate()->get();
    // Get user account
    foreach ($accounts as $object) {
    if ($object->id === $accountIds[0]) {
        $payer = $object;
    } elseif ($object->id === $accountIds[1]) {
        $payee = $object;
    }
    }
    $target = Auth::find(2);
    // Create trade
    $trade = $payer->beginTradeAmount($target);
    // Amount of expenses by trade
    $order = $trade->amountOfExpenses(100, [
            'label' => 'member-trade',
            'content' => 'EXPENSES',
            'note' => 'Custom notes 1'
            ]);
    $target = Auth::find(1);
    // Create trade and associate master order
    $trade = $payee->beginTradeAmount($target, $order);
    // Amount of income by trade order
    $order2 = $trade->amountOfIncome(100, [
            'label' => 'member-trade',
            'content' => 'INCOME',
            'note' => 'Custom notes 2'
            ]);
    // Related order
    // $order->order = $order2->order
    // $order->created_at = $order2->created_at
    DB::commit();
```

## 交易
trade_log
D:\taki\bet_api\app\Libraries\Traits\Entity\Trade\Auth.rd.php

## 單據
D:\taki\bet_api\app\Libraries\Traits\Entity\Receipt\Auth.rd.php

receipts

## 合併程式注意
1. token要對
2. token.login要移除


#要添加
D:\taki\bet_api\resources\lang\en\ban.php

---
## 測試用
php artisan make:command Tests/TestModelJob

// php artisan tests:model


UPDATE `lottery_game_bet` SET `user_id` = '2', `win_user` = '0'