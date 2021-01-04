# 公司文件 - 探索 - 遊戲專案

### 跟廠商串接測試小工具
```
https://lebo456.cc/_lab_/inum/launch.php
```

### API callback 301
修改Nginx的vhost

```
/etc/nginx/plesk.conf.d/vhosts
vi lebo456.cc.conf
```

```
        client_max_body_size 128m;
        // 添加在這下面,有兩處80與443

        location /callback/api/auth {
                try_files $uri $uri/index.php;
        }
```