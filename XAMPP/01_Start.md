# XAMPP

## 安裝PHP REDIS

### 查詢版本
    php -i | find "Architecture"
    php -i | find "Thread"

### 設定XAMPP
Apache > Config > PHP(php.ini)
```
[REDIS]
extension=php_redis.dll
```

### 下載DLL
[下載PHP REDIS DLL](https://windows.php.net/downloads/pecl/releases/redis/)

### 參考
[Windows 下安裝php redis擴充套件 - IT閱讀](https://www.itread01.com/content/1544590922.html)