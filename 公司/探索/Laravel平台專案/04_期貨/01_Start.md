# 公司文件 - 探索 - Laravel開發專案 - 期貨

## 使用工具
1. Laravel - V8.32.*
2. Laravel-admin - V1.*
3. Laravel websocket

## 前端
### 頁面
1. 前導頁(對接會員過來)
    - [API]-對接簽章登入會員
    - [API]-轉入點數
    - [API]-轉出點數
2. 首頁
    - [API]-商品清單(下拉選項用)
    1. 目標商品
        - [API]-單項商品每分鐘狀態(目標商品)
        - [WebSocket]-走勢圖每秒資料
    2. 會員資料，下注(大,小 切換 單,雙)
        - [API]-會員狀態
        - [API]-下注(單項)
    3. 商品每分鐘更新
        - [API]-全商品每分鐘狀態(全部)
    4. 走勢圖
        - [API]-商品每分鐘走勢(單項)
    5. K圖
        - [API]-商品歷史牌價(單項)
3. 下單紀錄
    - [API]-下單紀錄(全部或單項)
4. 商品說明
    - [API]-商品說明(單項)

### 前端對接流程
1. 平台會員帶入簽章
2. 後端透過HTTP去打平台取回UID
3. UID註冊成會員(密碼)
4. 直接登入
5. 選擇商品，看曲線圖
6. 下單(大,小 切換 單,雙)
    1. 檢查登入
    2. 檢查餘額
    3. 打API(返回會員資料)
7. 系統結算
8. 重複5.6.和7.

## 後端
### 功能目錄
1. 會員
2. 商品
3. 網站
    1. 公告(Bulletin)
    2. 經銷商(Dealer - 對接的key裝這)
    3. 系統配置(Config)
4. 系統
    1. 管理員
    2. 角色
    3. 權限
    4. 目錄
    5. log

### 資料庫
1. 網站配置(Website)
    - php artisan make:migration Website --path="database/migrations/20210319"
2. 經銷商(Dealer)
    - php artisan make:migration Dealer --path="database/migrations/20210319"
3. 會員(Member)
    - php artisan make:migration Member --path="database/migrations/20210319"
4. 會員-點數(Member_Point)
    - php artisan make:migration MemberPoint --path="database/migrations/20210319"
5. 商品(Binary)
    - php artisan make:migration Binary --path="database/migrations/20210319"
6. 商品-開獎期-投注統計(Binary_Draw)
    - php artisan make:migration BinaryDraw --path="database/migrations/20210319"
7. 會員-投注紀錄(Member_Betting)
    - php artisan make:migration MemberBetting --path="database/migrations/20210319"
8. 會員-點數-異動紀錄(Member_Point_Logs)
    - php artisan make:migration MemberPointLogs --path="database/migrations/20210319"
9. 公告(Bulletin)
    - php artisan make:migration Bulletin --path="database/migrations/20210319"

### 遊戲排程
1. 去抓期權資料
2. 生成每日期數
2. 產生期數資料
3. 兌換中獎結果

### 全球虛擬貨幣資料拉取
1. 參考網站
    - [Nomics](https://p.nomics.com)
    - [Nomics API DOC](https://nomics.com/docs/#operation/getCurrenciesTicker)
2. 註冊API-Key
```
API key: fcfcc6b5f6937b8f50d506861af1ecbb
Email: will@taki.tw
```

### WebSocket
