# 公司文件 - 探索 - Laravel開發專案 - 平台 - 開發紀錄

## 註冊會員(Member)流程
1. 取得Client Token(Global Service)             // Auth > Get Token
2. 取得簡訊                                     // Member > Auth > Verify Code
3. 切換Client Token(Member Service)             // Auth > Get Token
4. 註冊會員(PS.不是電話登入)                    // Member > Auth > Member Logon
5. 登入會員(Client Token會被銷毀要重新登入)     // Auth > User Login
