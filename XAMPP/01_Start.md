# XAMPP

## 安裝
[下載檔案](https://www.apachefriends.org/zh_tw/download.html)安裝
```
    7.4.13 / PHP 7.4.13
```
建議版本

---

## 相關目錄

### PHP目錄
C:\xampp\php

### 工作目錄
C:\xampp\htdocs

--

## 安裝REDIS
[下載檔案](https://github.com/microsoftarchive/redis/releases)安裝

---

## 配置PHP的REDIS

### 查詢版本
```
    php -i | find "Architecture"
    php -i | find "Thread"
```
### 下載DLL
[下載PHP REDIS DLL](https://windows.php.net/downloads/pecl/releases/redis/)
```
    php_redis-5.3.2-7.4-ts-vc15-x64
```
建議版本
```
    C:\xampp\php\ext
```
檔案放置位置

### 設定XAMPP
Apache > Config > PHP(php.ini)
```
[REDIS]
extension=php_redis.dll
```

### 參考
[Windows 下安裝php redis擴充套件 - IT閱讀](https://www.itread01.com/content/1544590922.html)

---

## PHP Redis Admin
[下載檔案](https://github.com/erikdubbelboer/phpRedisAdmin)

解壓縮

複製放到C:\xampp\htdocs

composer install

---

## PHP Email
使用[mailtrap](https://mailtrap.io/)中介


