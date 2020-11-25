# XAMPP

## 安裝PHP REDIS

### 查詢版本
    php -i | find "Architecture"
    php -i | find "Thread"

### 下載DLL
[下載PHP REDIS DLL](https://windows.php.net/downloads/pecl/releases/redis/)

    php_redis-5.3.2-7.4-ts-vc15-x64
建議版本

    C:\xampp\php\ext
檔案放置位置

### 設定XAMPP
Apache > Config > PHP(php.ini)
```
[REDIS]
extension=php_redis.dll
```

### 參考
[Windows 下安裝php redis擴充套件 - IT閱讀](https://www.itread01.com/content/1544590922.html)