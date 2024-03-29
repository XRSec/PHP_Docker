# [PHP-FPM_Docker](https://xrsec.vercel.app/PHP%20FPM%20Docker.html)

![version](https://img.shields.io/badge/Version-PHP%207.4-da282a)  ![version](https://img.shields.io/badge/Version-PHP%205.6-da282a)  [![Docker Automated Build](https://img.shields.io/docker/automated/xrsec/php?label=Build&logo=docker&style=flat-square)](https://hub.docker.com/r/xrsec/php) [![Docker PHP Build](https://github.com/XRSec/PHP_Docker/actions/workflows/Docker_PHP_Build.yml/badge.svg)](https://github.com/XRSec/PHP_Docker/actions/workflows/Docker_PHP_Build.yml)

> Please compile manually, github action may time out

## USE

```dockerfile
FROM xrsec/php:latest | FROM xrsec/php:5.6 | FROM xrsec/php:7.4 | FROM xrsec/php:init
LABEL From="xrsec"
ARG TARGETPLATFORM
```

```dockerfile
RUN mkdir -p /www /www/server /www/bak /www/server/php74 /www/server/php56
COPY --from=xrsec/php:7.4 /www/server/php74 /www/server/php74
COPY --from=xrsec/php:5.6 /www/server/php56 /www/server/php56
```

```dockerfile
RUN ln -sf /www/server/php74/bin/php /www/env/php74 \
    && ln -sf /www/server/php74/sbin/php-fpm /www/env/php74-fpm \
    && ln -sf /www/server/php74/bin/pecl /www/env/php74-pecl \
    && ln -sf /www/server/php74/bin/pear /www/env/php74-pear \
    && rm -rf /usr/bin/php74 \
    && rm -rf /usr/bin/php74-fpm \
    && rm -rf /usr/bin/php74-pecl \
    && rm -rf /usr/bin/php74-pear

# PHP56 configuration files
RUN ln -sf /www/server/php56/sbin/php-fpm /www/env/php56-fpm \
    && ln -sf /www/server/php56/bin/php /www/env/php56 \
    && ln -sf /www/server/php56/bin/pecl /www/env/php56-pecl \
    && ln -sf /www/server/php56/bin/pear /www/env/php56-pear \
    && rm -rf /usr/bin/php56-fpm \
    && rm -rf /usr/bin/php56 \
    && rm -rf /usr/bin/php56-pecl \
    && rm -rf /usr/bin/php56-pear
```

## Configuration

### Basic information

Optional version：`PHP 5.6.40 & FPM`  `PHP 7.4.16 & FPM`  `PHP 5.6.40 & PHP 7.4.16 & FPM`

```ini
/www/server/php74/bin/php
/www/server/php74/sbin/php-fpm
/www/server/php74/lib/php.ini
/www/server/php56/bin/php
/www/server/php56/sbin/php-fpm
/www/server/php56/lib/php.ini
 php-debug = 9003
 user = nginx
```

### Xdebug 2.5.5

```ini
zend_extension=/www/server/php56/lib/php/extensions/no-debug-non-zts-20131226/xdebug.so
xdebug.remote_enable = on
xdebug.remote_host = docker.for.mac.localhost
xdebug.remote_port = 9003
xdebug.idekey="PHPSTORM"
xdebug.remote_autostart = 1
xdebug.remote_connect_back = 1
xdebug.remote_handler = "dbgp"
```

### Xdebug 3.0.0

```ini
zend_extension=/www/server/php74/lib/php/extensions/no-debug-non-zts-20190902/xdebug.so
xdebug.mode = debug
debug.remote_handler = dbgp
xdebug.client_host = docker.for.mac.localhost
xdebug.start_with_request = yes
xdebug.client_port = 9003
xdebug.idekey = "PHPSTORM"
```

> XRSec has the right to modify and interpret this article. If you want to reprint or disseminate this article, you must ensure the integrity of this article, including all contents such as copyright notice. Without the permission of the author, the content of this article shall not be modified or increased or decreased arbitrarily, and it shall not be used for commercial purposes in any way
