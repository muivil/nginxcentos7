# Обновление NGINX до версии 1.30.7 для BitrixVM 7 (CentOS 7)

## Описание
Пакеты для сборки и specs в докере если захотите собрать сами (nginx_dockerbuid.7z) 

Плюс последний bx-nginx-1.24.0-0.el7.ngx.src.rpm(если захотите сами взять от туда пакеты)

оттуда был взят nginx-push-stream-module-0.5.6.tgz

Готовый bx-nginx-1.30.7-0.el7.ngx.x86_64.rpm 

Ну и не забываем забэкапить /etc/nginx и бинарник на всякий случай /usr/sbin/nginx

До обновления скачайте свою версию из репозитория что бы можно было откатится если что то пойдет не так.

У меня была 1.20.2

yumdownloader bx-nginx-1.20.2-0.el7.ngx.x86_64

## Информация о сборке
```
nginx -V
nginx version: nginx/1.30.7
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC)
built with OpenSSL 1.1.1w  11 Sep 2023
TLS SNI support enabled
configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --conf-path=/etc/nginx/nginx.conf
--error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid
--lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp
--http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp
--http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp
--user=nginx --group=nginx --with-openssl=/root/rpmbuild/BUILD/bx-nginx-1.30.7/openssl-1.1.1w
--with-openssl-opt=enable-tls1_3 --with-http_ssl_module --with-http_realip_module --with-http_addition_module
--with-http_sub_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module
--with-http_gunzip_module --with-http_gzip_static_module --with-http_random_index_module
--with-http_secure_link_module --with-http_stub_status_module --with-http_auth_request_module
--with-http_v2_module --with-mail --with-mail_ssl_module --with-file-aio
--add-module=/root/rpmbuild/BUILD/bx-nginx-1.30.7/nginx-push-stream-module-0.5.6
--add-module=/root/rpmbuild/BUILD/bx-nginx-1.30.7/mod_zip-1.3.0
--add-module=/root/rpmbuild/BUILD/bx-nginx-1.30.7/headers-more-nginx-module-0.34
--add-module=/root/rpmbuild/BUILD/bx-nginx-1.30.7/ngx_brotli --with-cc-opt='-O2
-g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong
--param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic'
```
## Установка
Скачайте пакет в releases

или wget https://github.com/muivil/nginxcentos7/releases/download/bx-nginx-1.30.7-0.el7.ngx.x86_64/bx-nginx-1.30.7-0.el7.ngx.x86_64.rpm

Обновление можно сделать так
```
rpm -Uvh bx-nginx-1.30.7-0.el7.ngx.x86_64.rpm
```
После обновления проверьте конфиги и версию
```
nginx -t
nginx -v
```

## Изменение конфигурации

Начиная с 1.25.0 параметр http2 необходимо указывать отдельно.

Было:
```
server {
    listen 443 ssl http2;
}
```
Стало:
```
server {
    listen 443 ssl;
    http2 on;
}
```
Проверьте конфигурацию:
```
nginx -t
```
Если ошибок нет, перезапустите nginx:
```
systemctl restart nginx
```
