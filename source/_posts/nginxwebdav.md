---
layout: post
title: "nginx-webdav"
excerpt: "NGINX WebDAV 설정"
category: program
publish_date: "2015-07-24"
date: "2015-07-24"
tags: [nginx, nginx webdav, webdav, dav]

author_url: /author/deptno
author_avatar: deptno-yuri
show_avatar: true
show_related_posts: true
feature_image: ubuntu
square_related: ubuntu
read_time: 5
comments: true
---

## NGINX WebDAV setup

nginx에 모듈을 추가하기 위해 빌드를 다시 해야하므로 nginx stable 버전 source와 nginx-dav-ext-module을 다운로드 받아 압축을 푼다.

* [nginx](http://nginx.org/)
* [nginx-dav-ext-module](https://github.com/arut/nginx-dav-ext-module/)

기존 설치되어 있는 nginx의 설정을 복사하기 위해 아래 명령어를 실행하고 --with부터 복사를 해둔다.

``` bash
bglee@deptno:~/tmp/nginx/nginx-1.8.0$ nginx -V
nginx version: nginx/1.4.6 (Ubuntu)
built by gcc 4.8.2 (Ubuntu/Linaro 4.8.2-19ubuntu1)
TLS SNI support enabled
configure arguments: --with-cc-opt='-g -O2 -fstack-protector --param=ssp-buffer-size=4 -Wformat -Werror=format-security -D_FORTIFY_SOURCE=2' --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro' --prefix=/usr/share/nginx --conf-path=/etc/nginx/nginx.conf --http-log-path=/var/log/nginx/access.log --error-log-path=/var/log/nginx/error.log --lock-path=/var/lock/nginx.lock --pid-path=/run/nginx.pid --http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-proxy-temp-path=/var/lib/nginx/proxy --http-scgi-temp-path=/var/lib/nginx/scgi --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --with-debug --with-pcre-jit --with-ipv6 --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module --with-http_addition_module --with-http_dav_module --with-http_geoip_module --with-http_gzip_static_module --with-http_image_filter_module --with-http_spdy_module --with-http_sub_module --with-http_xslt_module --with-mail --with-mail_ssl_module
```

압축을 해제한 nginx source 디렉토리에서 아래와 같이 configure를 설정한다

``` bash
bglee@deptno:~/tmp/nginx/nginx-1.8.0$ ./configure --prefix=/etc/nginx --conf-path=/etc/nginx/nginx.conf --sbin-path=/usr/sbin/nginx --pid-path=/var/run/nginx.pid --lock-path=/var/lock/nginx.lock --user=http --group=http --http-log-path=/var/log/nginx/access.log --error-log-path=/var/log/nginx/error.log --http-client-body-temp-path=/var/lib/nginx/client-body --http-proxy-temp-path=/var/lib/nginx/proxy --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-scgi-temp-path=/var/lib/nginx/scgi --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --with-imap --with-imap_ssl_module --with-ipv6 --with-pcre-jit --with-file-aio --with-http_dav_module --add-module=/home/bglee/tmp/nginx/nginx-dav-ext-module-master --with-http_geoip_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_realip_module --with-http_spdy_module --with-http_ssl_module --with-http_stub_status_module
bglee@deptno:~/tmp/nginx/nginx-1.8.0$ service nginx start
```

설정이 끝났으면 빌드와 설치를 하기 위해 아래와 같이 입력한다

``` bash
bglee@deptno:~/tmp/nginx/nginx-1.8.0$ make && make install
bglee@deptno:~/tmp/nginx/nginx-1.8.0$ service nginx start
```

>빌드 오류일 경우 `make`에서, 퍼미션 오류일 경우 `make install`이 nginx를 설치하는 과정에 났다고 보면된다.
전자의 경우는 구글에서 솔루션으르 검색하고 후자의 경우는 `sudo make install`로 설치하면 해결된다

마지막으로 nginx 구동을 위해 아래 명령어를 실행하고 퍼미션 오류의 경우는 `sudo`를 붙여주면 된다.

``` bash
bglee@deptno:~/tmp/nginx/nginx-1.8.0$ service nginx stop
bglee@deptno:~/tmp/nginx/nginx-1.8.0$ service nginx reload
bglee@deptno:~/tmp/nginx/nginx-1.8.0$ service nginx start
```

인증을 통해 제공하려면 <https://www.digitalocean.com/community/tutorials/how-to-set-up-http-authentication-with-nginx-on-ubuntu-12-10>를 참고한다.
