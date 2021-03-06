# 公司文件 - 探索 - Laravel開發專案 - 彩卷

## 版本
Laravel 7.22

PHP 7.4

使用L5 Repositories

---

## 安裝
```cmd
git pull
composer install
// 需要安裝`Redis`
php artisan migrate
php artisan db:seed
php artisan data:client-read
// 1294583
php artisan data:client-add
// Admin Service, 1, yes
php artisan data:client-add
// Member Service, 2, yes
php artisan data:client-add
// Server Service, 3, yes
php artisan data:client-add
// Business Service, 4, yes
```

## 規劃
### 系統面
1. 統一管理同類型彩票遊戲
2. 彩票遊戲透過規則來玩,投注方式都遊規則產生
3. 預開號碼跟中獎規則,統計排率去做調整
4. 發獎勵彩票時間道在發送

### 點數種類
Gold - (一般點數)
Gift - (禮品點數,反水,贈點)

### API面
1. 系統設定
    1. 環境設定客戶(時區)
2. 後台管理層面
    1. 查看遊戲項目
        1. 清單
        2. 新增
        3. 看
        4. 修改
    2. 查看各遊戲規則分類
        1. 清單
        2. 新增
        3. 看
        4. 修改
    3. 查看各遊戲規則
        1. 清單
        2. 新增
        3. 看
        4. 修改
    4. 查看各遊戲開獎號碼
        1. 清單
        2. 重開(還未到開獎時間的)
    5. 查看下注訂單
        1. 清單
3. 前台會員層面
    1. 會員
        1. 註冊or登入
        2. 驗證碼登入
        3. 入點
        4. 出點
    2. 遊玩彩票遊戲
        1. 遊戲規則(下注用)
        2. 下注
            (點數-紀錄)
    3. 查看資訊
        1. 遊戲介紹
        2. 遊戲規則介紹
        3. 開獎歷史
        4. 我的下注紀錄
4. 排程
    1. 每天跑開獎期
    2. 預開獎
    3. 配獎
        (點數-紀錄)
    4. 反水
---

## 系統建構
php artisan make:currency
### Migration
```php
// php artisan make:migration create_lottery_game_settings_table
// php artisan make:migration create_lottery_game_rules_table
// php artisan make:migration create_lottery_game_draws_table
// php artisan make:migration create_lottery_game_orders_table
```
`PS:Entity指令可以一併建立Migration所以不自行建立`

### Entity
```php
php artisan make:entity LotteryGames/GameSetting
php artisan make:entity LotteryGames/GameRule
php artisan make:entity LotteryGames/GameRuleType
php artisan make:entity LotteryGames/GameDraw
php artisan make:entity LotteryGames/GameBet
```
全部選yes

### Seeder
```php
php artisan make:seeder LotteryGameSeeder
```

### Job 建立排程
```php
// 建立遊戲每先所有開獎期數
php artisan make:command LotteryGames/Lottery01/GamePeriodsJob
// php artisan games-lottery:periods

// 預開開獎期數
php artisan make:command LotteryGames/Lottery01/GameDrawJob
// php artisan games-lottery:draw

// 開獎時間到分配獎勵
php artisan make:command LotteryGames/Lottery01/GameRewardJob
// php artisan games-lottery:reward

// 遊戲回饋點數-贈點
php artisan make:command LotteryGames/Lottery01/GameGiveBackJob
// php artisan games-lottery:GiveBack

// 測試用
// php artisan tests:model
```

---

## 表單設計

### 彩票遊戲設定
```php
```

### 彩票遊戲規則
```php
```

### 彩票遊戲開獎
```php
```

### 彩票遊戲訂單
```php
```

---

## 測試模擬開獎
```php
> php artisan migrate:rollback
> php artisan migrate
> php artisan db:seed --class=LotteryGameSeeder
> php artisan games-lottery:periods
> php artisan games-lottery:draw
> php artisan games-lottery:reward
```

---

## 遊戲流程
遊戲開始

猜號

封盤(停止下注)

算聖率

開獎
```
$no_arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
$open_arr = [];
while(count($no_arr) >= 1){
    shuffle($no_arr);
    array_push($open_arr, array_shift($no_arr));
}
echo '開獎結果';
print_r($open_arr);
```

統計

## 
遊戲項目
遊戲規則
遊戲開號紀錄
遊戲投放紀錄
遊戲發放紀錄


每天.每小時.每分鐘
查詢(遊戲項目設定,現在時間是你的周期 且 是你的營業時段內)
開獎 OR 等下一個排成

配獎
每天.每小時.每分鐘
查詢(遊戲開號紀錄,啟用 且 未派發)
查詢中獎規則(JSON)符合遊戲投放紀錄的資料
發送

## 資料表
遊戲設定GameItemSetting
id,
名稱,
一般區(開獎資料),
一般區(開獎位數),
一般區(重複),
特別區(開獎資料),
特別區(開獎位數),
特別區(重複),
週期(星期幾),
開始時間,
結束時間,
間格,
截止時間,
預約(提早開出),
贏率,
狀態,
建立時間,
更新時間,

遊戲規則項目GameRule
id,
遊戲設定_id,
規則描述,
規則JSON,
賠率,
排序,
狀態,
建立時間,
更新時間,

INSERT INTO `cp_dmodel` VALUES 
(0,'0','','',0,0,50,'冠军',NULL,'',0,1,0,''),
(1,'0','','',0,0,50,'亚军',NULL,'',0,2,0,''),
(2,'0','','',0,0,50,'第三名',NULL,'',0,3,0,''),
(3,'0','','',0,0,50,'第四名',NULL,'',0,4,0,''),
(4,'0','','',0,0,50,'第五名',NULL,'',0,5,0,''),
(5,'0','','',0,0,50,'第六名',NULL,'',0,6,0,''),
(6,'0','','',0,0,50,'第七名',NULL,'',0,7,0,''),
(7,'0','','',0,0,50,'第八名',NULL,'',0,8,0,''),
(8,'0','','',0,0,50,'第九名',NULL,'',0,9,0,''),
(9,'0','','',0,0,50,'第十名',NULL,'',0,10,0,''),

(10,'0','','',0,0,50,'大',NULL,'1.995',501101,1665,0,'冠军 大'),
(11,'0','','',0,0,50,'小',NULL,'1.995',501102,1666,0,'冠军 小'),
(12,'0','','',0,0,50,'单',NULL,'1.995',501103,1667,0,'冠军 单'),
(13,'0','','',0,0,50,'双',NULL,'1.995',501104,1668,0,'冠军 双'),
(14,'0','','',0,0,50,'龙',NULL,'1.995',501105,1669,0,'冠军 龙'),
(15,'0','','',0,0,50,'虎',NULL,'1.995',501106,1670,0,'冠军 虎'),

(16,'0','','',0,0,50,'大',NULL,'1.995',501201,1671,0,'亚军 大'),
(17,'0','','',0,0,50,'小',NULL,'1.995',501202,1672,0,'亚军 小'),
(18,'0','','',0,0,50,'单',NULL,'1.995',501203,1673,0,'亚军 单'),
(19,'0','','',0,0,50,'双',NULL,'1.995',501204,1674,0,'亚军 双'),
(20,'0','','',0,0,50,'龙',NULL,'1.995',501205,1675,0,'亚军 龙'),
(21,'0','','',0,0,50,'虎',NULL,'1.995',501206,1676,0,'亚军 虎'),

(22,'0','','',0,0,50,'大',NULL,'1.995',501301,1677,0,'第三名 大'),
(23,'0','','',0,0,50,'小',NULL,'1.995',501302,1678,0,'第三名 小'),
(24,'0','','',0,0,50,'单',NULL,'1.995',501303,1679,0,'第三名 单'),
(25,'0','','',0,0,50,'双',NULL,'1.995',501304,1680,0,'第三名 双'),
(26,'0','','',0,0,50,'龙',NULL,'1.995',501305,1681,0,'第三名 龙'),
(27,'0','','',0,0,50,'虎',NULL,'1.995',501306,1682,0,'第三名 虎'),

(34,'0','','',0,0,50,'大',NULL,'1.995',501401,1683,0,'第四名 大'),
(35,'0','','',0,0,50,'小',NULL,'1.995',501402,1684,0,'第四名 小'),
(36,'0','','',0,0,50,'单',NULL,'1.995',501403,1685,0,'第四名 单'),
(37,'0','','',0,0,50,'双',NULL,'1.995',501404,1686,0,'第四名 双'),
(38,'0','','',0,0,50,'龙',NULL,'1.995',501405,1687,0,'第四名 龙'),
(39,'0','','',0,0,50,'虎',NULL,'1.995',501406,1688,0,'第四名 虎'),

(40,'0','','',0,0,50,'大',NULL,'1.995',501501,1689,0,'第五名 大'),
(41,'0','','',0,0,50,'小',NULL,'1.995',501502,1690,0,'第五名 小'),
(42,'0','','',0,0,50,'单',NULL,'1.995',501503,1691,0,'第五名 单'),
(43,'0','','',0,0,50,'双',NULL,'1.995',501504,1692,0,'第五名 双'),
(44,'0','','',0,0,50,'龙',NULL,'1.995',501505,1693,0,'第五名 龙'),

(45,'0','','',0,0,50,'虎',NULL,'1.995',501506,1694,0,'第六名 虎'),
(46,'0','','',0,0,50,'大',NULL,'1.995',501601,1695,0,'第六名 大'),
(47,'0','','',0,0,50,'小',NULL,'1.995',501602,1696,0,'第六名 小'),
(48,'0','','',0,0,50,'单',NULL,'1.995',501603,1697,0,'第六名 单'),
(49,'0','','',0,0,50,'双',NULL,'1.995',501604,1698,0,'第六名 双'),

(52,'0','','',0,0,50,'大',NULL,'1.995',501701,1701,0,'第七名 大'),
(53,'0','','',0,0,50,'小',NULL,'1.995',501702,1702,0,'第七名 小'),
(54,'0','','',0,0,50,'单',NULL,'1.995',501703,1703,0,'第七名 单'),
(55,'0','','',0,0,50,'双',NULL,'1.995',501704,1704,0,'第七名 双'),

(58,'0','','',0,0,50,'大',NULL,'1.995',501801,1701,0,'第八名 大'),
(59,'0','','',0,0,50,'小',NULL,'1.995',501802,1702,0,'第八名 小'),
(60,'0','','',0,0,50,'单',NULL,'1.995',501803,1703,0,'第八名 单'),
(61,'0','','',0,0,50,'双',NULL,'1.995',501804,1704,0,'第八名 双'),

(64,'0','','',0,0,50,'大',NULL,'1.995',501901,1701,0,'第九名 大'),
(65,'0','','',0,0,50,'小',NULL,'1.995',501902,1702,0,'第九名 小'),
(66,'0','','',0,0,50,'单',NULL,'1.995',501903,1703,0,'第九名 单'),
(67,'0','','',0,0,50,'双',NULL,'1.995',501904,1704,0,'第九名 双'),

(70,'0','','',0,0,50,'大',NULL,'1.995',502001,1701,0,'第十名 大'),
(71,'0','','',0,0,50,'小',NULL,'1.995',502002,1702,0,'第十名 小'),
(72,'0','','',0,0,50,'单',NULL,'1.995',502003,1703,0,'第十名 单'),
(73,'0','','',0,0,50,'双',NULL,'1.995',502004,1704,0,'第十名 双'),

(74,'0','','',1,0,50,'1',1,'9.95',501107,1701,0,'冠军 1'),
(75,'0','','',1,0,50,'2',2,'9.95',501108,1702,0,'冠军 2'),
(76,'0','','',1,0,50,'3',3,'9.95',501109,1703,0,'冠军 3'),
(77,'0','','',1,0,50,'4',4,'9.95',501110,1704,0,'冠军 4'),
(78,'0','','',1,0,50,'5',5,'9.95',501111,1701,0,'冠军 5'),
(79,'0','','',1,0,50,'6',6,'9.95',501112,1702,0,'冠军 6'),
(80,'0','','',1,0,50,'7',7,'9.95',501113,1703,0,'冠军 7'),
(81,'0','','',1,0,50,'8',8,'9.95',501114,1704,0,'冠军 8'),
(82,'0','','',1,0,50,'9',9,'9.95',501115,1703,0,'冠军 9'),
(83,'0','','',1,0,50,'10',10,'9.95',501116,1704,0,'冠军10'),

(84,'0','','',1,0,50,'1',1,'9.95',501207,1701,0,'亚军 1'),
(85,'0','','',1,0,50,'2',2,'9.95',501208,1702,0,'亚军 2'),
(86,'0','','',1,0,50,'3',3,'9.95',501209,1703,0,'亚军 3'),
(87,'0','','',1,0,50,'4',4,'9.95',501210,1704,0,'亚军 4'),
(88,'0','','',1,0,50,'5',5,'9.95',501211,1701,0,'亚军 5'),
(89,'0','','',1,0,50,'6',6,'9.95',501212,1702,0,'亚军 6'),
(90,'0','','',1,0,50,'7',7,'9.95',501213,1703,0,'亚军 7'),
(91,'0','','',1,0,50,'8',8,'9.95',501214,1704,0,'亚军 8'),
(92,'0','','',1,0,50,'9',9,'9.95',501215,1703,0,'亚军 9'),
(93,'0','','',1,0,50,'10',10,'9.95',501216,1704,0,'亚军10'),

(94,'0','','',1,0,50,'1',1,'9.95',501307,1701,0,'第三名 1'),
(95,'0','','',1,0,50,'2',2,'9.95',501308,1702,0,'第三名 2'),
(96,'0','','',1,0,50,'3',3,'9.95',501309,1703,0,'第三名 3'),
(97,'0','','',1,0,50,'4',4,'9.95',501310,1704,0,'第三名 4'),
(98,'0','','',1,0,50,'5',5,'9.95',501311,1701,0,'第三名 5'),
(99,'0','','',1,0,50,'6',6,'9.95',501312,1702,0,'第三名 6'),
(100,'0','','',1,0,50,'7',7,'9.95',501313,1703,0,'第三名 7'),
(101,'0','','',1,0,50,'8',8,'9.95',501314,1704,0,'第三名 8'),
(102,'0','','',1,0,50,'9',9,'9.95',501315,1703,0,'第三名 9'),
(103,'0','','',1,0,50,'10',10,'9.95',501316,1704,0,'第三名10'),

(104,'0','','',1,0,50,'1',1,'9.95',501407,1701,0,'第四名 1'),
(105,'0','','',1,0,50,'2',2,'9.95',501408,1702,0,'第四名 2'),
(106,'0','','',1,0,50,'3',3,'9.95',501409,1703,0,'第四名 3'),
(107,'0','','',1,0,50,'4',4,'9.95',501410,1704,0,'第四名 4'),
(108,'0','','',1,0,50,'5',5,'9.95',501411,1701,0,'第四名 5'),
(109,'0','','',1,0,50,'6',6,'9.95',501412,1702,0,'第四名 6'),
(110,'0','','',1,0,50,'7',7,'9.95',501413,1703,0,'第四名 7'),
(111,'0','','',1,0,50,'8',8,'9.95',501414,1704,0,'第四名 8'),
(112,'0','','',1,0,50,'9',9,'9.95',501415,1703,0,'第四名 9'),
(113,'0','','',1,0,50,'10',10,'9.95',501416,1704,0,'第四名10'),

(114,'0','','',1,0,50,'1',1,'9.95',501507,1701,0,'第五名 1'),
(115,'0','','',1,0,50,'2',2,'9.95',501508,1702,0,'第五名 2'),
(116,'0','','',1,0,50,'3',3,'9.95',501509,1703,0,'第五名 3'),
(117,'0','','',1,0,50,'4',4,'9.95',501510,1704,0,'第五名 4'),
(118,'0','','',1,0,50,'5',5,'9.95',501511,1701,0,'第五名 5'),
(119,'0','','',1,0,50,'6',6,'9.95',501512,1702,0,'第五名 6'),
(120,'0','','',1,0,50,'7',7,'9.95',501513,1703,0,'第五名 7'),
(121,'0','','',1,0,50,'8',8,'9.95',501514,1704,0,'第五名 8'),
(122,'0','','',1,0,50,'9',9,'9.95',501515,1703,0,'第五名 9'),
(123,'0','','',1,0,50,'10',10,'9.95',501516,1704,0,'第五名10'),

(124,'0','','',1,0,50,'1',1,'9.95',501607,1701,0,'第六名 1'),
(125,'0','','',1,0,50,'2',2,'9.95',501608,1702,0,'第六名 2'),
(126,'0','','',1,0,50,'3',3,'9.95',501609,1703,0,'第六名 3'),
(127,'0','','',1,0,50,'4',4,'9.95',501610,1704,0,'第六名 4'),
(128,'0','','',1,0,50,'5',5,'9.95',501611,1701,0,'第六名 5'),
(129,'0','','',1,0,50,'6',6,'9.95',501612,1702,0,'第六名 6'),
(130,'0','','',1,0,50,'7',7,'9.95',501613,1703,0,'第六名 7'),
(131,'0','','',1,0,50,'8',8,'9.95',501614,1704,0,'第六名 8'),
(132,'0','','',1,0,50,'9',9,'9.95',501615,1703,0,'第六名 9'),
(133,'0','','',1,0,50,'10',10,'9.95',501616,1704,0,'第六名10'),

(134,'0','','',1,0,50,'1',1,'9.95',501707,1701,0,'第七名 1'),
(135,'0','','',1,0,50,'2',2,'9.95',501708,1702,0,'第七名 2'),
(136,'0','','',1,0,50,'3',3,'9.95',501709,1703,0,'第七名 3'),
(137,'0','','',1,0,50,'4',4,'9.95',501710,1704,0,'第七名 4'),
(138,'0','','',1,0,50,'5',5,'9.95',501711,1701,0,'第七名 5'),
(139,'0','','',1,0,50,'6',6,'9.95',501712,1702,0,'第七名 6'),
(140,'0','','',1,0,50,'7',7,'9.95',501713,1703,0,'第七名 7'),
(141,'0','','',1,0,50,'8',8,'9.95',501714,1704,0,'第七名 8'),
(142,'0','','',1,0,50,'9',9,'9.95',501715,1703,0,'第七名 9'),
(143,'0','','',1,0,50,'10',10,'9.95',501716,1704,0,'第七名10'),

(144,'0','','',1,0,50,'1',1,'9.95',501807,1701,0,'第八名 1'),
(145,'0','','',1,0,50,'2',2,'9.95',501808,1702,0,'第八名 2'),
(146,'0','','',1,0,50,'3',3,'9.95',501809,1703,0,'第八名 3'),
(147,'0','','',1,0,50,'4',4,'9.95',501810,1704,0,'第八名 4'),
(148,'0','','',1,0,50,'5',5,'9.95',501811,1701,0,'第八名 5'),
(149,'0','','',1,0,50,'6',6,'9.95',501812,1702,0,'第八名 6'),
(150,'0','','',1,0,50,'7',7,'9.95',501813,1703,0,'第八名 7'),
(151,'0','','',1,0,50,'8',8,'9.95',501814,1704,0,'第八名 8'),
(152,'0','','',1,0,50,'9',9,'9.95',501815,1703,0,'第八名 9'),
(153,'0','','',1,0,50,'10',10,'9.95',501816,1704,0,'第八名10'),

(154,'0','','',1,0,50,'1',1,'9.95',501907,1701,0,'第九名 1'),
(155,'0','','',1,0,50,'2',2,'9.95',501908,1702,0,'第九名 2'),
(156,'0','','',1,0,50,'3',3,'9.95',501909,1703,0,'第九名 3'),
(157,'0','','',1,0,50,'4',4,'9.95',501910,1704,0,'第九名 4'),
(158,'0','','',1,0,50,'5',5,'9.95',501911,1701,0,'第九名 5'),
(159,'0','','',1,0,50,'6',6,'9.95',501912,1702,0,'第九名 6'),
(160,'0','','',1,0,50,'7',7,'9.95',501913,1703,0,'第九名 7'),
(161,'0','','',1,0,50,'8',8,'9.95',501914,1704,0,'第九名 8'),
(162,'0','','',1,0,50,'9',9,'9.95',501915,1703,0,'第九名 9'),
(163,'0','','',1,0,50,'10',10,'9.95',501916,1704,0,'第九名10'),

(164,'0','','',1,0,50,'1',1,'9.95',502007,1701,0,'第十名 1'),
(165,'0','','',1,0,50,'2',2,'9.95',502008,1702,0,'第十名 2'),
(166,'0','','',1,0,50,'3',3,'9.95',502009,1703,0,'第十名 3'),
(167,'0','','',1,0,50,'4',4,'9.95',502010,1704,0,'第十名 4'),
(168,'0','','',1,0,50,'5',5,'9.95',502011,1701,0,'第十名 5'),
(169,'0','','',1,0,50,'6',6,'9.95',502012,1702,0,'第十名 6'),
(170,'0','','',1,0,50,'7',7,'9.95',502013,1703,0,'第十名 7'),
(171,'0','','',1,0,50,'8',8,'9.95',502014,1704,0,'第十名 8'),
(172,'0','','',1,0,50,'9',9,'9.95',502015,1703,0,'第十名 9'),
(173,'0','','',1,0,50,'10',10,'9.95',502016,1704,0,'第十名10');

遊戲開獎紀錄GameDraw
id,
遊戲設定_id,
開獎期數,
一般區(開獎),
特別區(開獎),
中獎規則,
狀態,
建立時間,
更新時間,

會員下注紀錄GameOrder
id,
會員_id,
遊戲設定_id,
遊戲規則_id,
下注數值,
金額,
賠率,
已換算賠率數量,
輸贏,
狀態,
建立時間,
更新時間,





Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Games.md
        app/Console/Commands/LotteryGames/
        app/Http/Middleware/Demo/
        app/Libraries/LotteryGames/
        database/migrations/2020_12_04_090402_create_lottery_game_setting.php
        database/migrations/2020_12_04_090414_create_lottery_game_rule.php
        database/migrations/2020_12_04_090425_create_lottery_game_draw_log.php
        database/migrations/2020_12_04_090437_create_lottery_game_order.php
        database/seeds/GameSeeder.php


Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   app/Providers/RepositoryServiceProvider.php
        modified:   config/exception.php

Untracked files:
  (use "git add <file>..." to include in what will be committed)
            Games.md
            app/Console/Commands/LotteryGames/
        app/Entities/LotteryGames/
        app/Exceptions/LotteryGames/
        app/Http/Controllers/LotteryGames/
            app/Http/Middleware/Demo/
        app/Http/Requests/LotteryGames/
        app/Http/Responses/LotteryGames/
            app/Libraries/LotteryGames/
        app/Presenters/LotteryGames/
        app/Repositories/LotteryGames/
        app/Transformers/LotteryGames/
        app/Validators/LotteryGames/
            database/migrations/2020_12_04_090402_create_lottery_game_setting.php
            database/migrations/2020_12_04_090414_create_lottery_game_rule.php
            database/migrations/2020_12_04_090425_create_lottery_game_draw_log.php
            database/migrations/2020_12_04_090437_create_lottery_game_order.php
        database/migrations/2020_12_09_031258_create_lottery_games_game_settings_table.php
            database/seeds/GameSeeder.php
        resources/lang/en/exception/App/Exceptions/LotteryGames/


Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   app/Providers/RepositoryServiceProvider.php
        modified:   config/exception.php

Untracked files:
  (use "git add <file>..." to include in what will be committed)
            Games.md
            app/Console/Commands/LotteryGames/
        app/Entities/LotteryGames/
        app/Exceptions/LotteryGames/
        app/Http/Controllers/LotteryGames/
            app/Http/Middleware/Demo/
        app/Http/Requests/LotteryGames/
        app/Http/Responses/LotteryGames/
            app/Libraries/LotteryGames/
        app/Presenters/LotteryGames/
        app/Repositories/LotteryGames/
        app/Transformers/LotteryGames/
        app/Validators/LotteryGames/
            database/migrations/2020_12_04_090402_create_lottery_game_setting.php
            database/migrations/2020_12_04_090414_create_lottery_game_rule.php
            database/migrations/2020_12_04_090425_create_lottery_game_draw_log.php
            database/migrations/2020_12_04_090437_create_lottery_game_order.php
        database/migrations/2020_12_09_031258_create_lottery_games_game_settings_table.php
        database/migrations/2020_12_09_033650_create_lottery_games_game_rules_table.php
        database/migrations/2020_12_09_033658_create_lottery_games_game_draw_logs_table.php
        database/migrations/2020_12_09_033707_create_lottery_games_game_orders_table.php
            database/seeds/GameSeeder.php
        resources/lang/en/exception/App/Exceptions/LotteryGames/

php artisan make:command Tests/TestMoselJob
// php artisan my-test:model