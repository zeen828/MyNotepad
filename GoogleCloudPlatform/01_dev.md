# Google Cloud Platform

> # 服務器(Compute Engine)
> ## 個人測試機
> ### OS
> CentOS Linux
> 
> ### 主機名稱與ID
> demo-centos
>
> asia-east1-b
> 
> ### 服務應用
> Nginx V1.16.1
> 
> PHP V7.3.26
> ```
> sudo systemctl restart php56-php-fpm
> sudo systemctl restart php73-php-fpm
> sudo systemctl restart nginx
> ```
> 
> ### 網路服務器(Nginx)
> 設定檔位置:/etc/nginx/sites-enabled
> 
> 檔案位置:/usr/share/nginx/website/
> 
> ### PHP
> ```
> /etc/opt/remi/php73/php.ini
> /var/opt/remi/php73/lib/php/session	
> ```
> 
> ### phpMyAdmin
> 故障修理
> sudo chmod -R 777 /var/opt/remi/php73/lib/php/session/ -R
