# ========================================
# Apache用のCentOSイメージ
# ========================================
FROM centos:8

# CentOSを最新に
RUN dnf clean all
RUN dnf -y update
RUN dnf -y upgrade
RUN dnf -y install dnf-utils

RUN dnf -y install glibc-locale-source glibc-langpack-en
RUN localedef -f UTF-8 -i ja_JP ja_JP.UTF-8
ENV LANG="ja_JP.UTF-8" \
    LANGUAGE="ja_JP:ja" \
    LC_ALL="ja_JP.UTF-8"

# タイムゾーンをJSTに
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# todo:プロジェクトに合わせて適宜調整
# ＜PHP 8.0のインストール＞
# PHPリポジトリを有効にする
RUN dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
RUN dnf install -y https://rpms.remirepo.net/enterprise/remi-release-8.rpm
RUN dnf module enable php:remi-8.0 -y
# PHPおよび必要に応じて追加モジュールをインストールします。
RUN dnf -y install php
RUN dnf -y install php-mbstring
RUN dnf -y install php-zip
RUN dnf -y install php-gd
RUN dnf -y install php-xml
RUN dnf -y install php-opcache
RUN dnf -y install php-fpm
# MySQL/MariaDB用のモジュールをインストール
RUN dnf -y install php-mysqli
RUN dnf -y install php-mysqlnd
# PostgreSQL用のモジュールをインストール
RUN dnf install -y php-pgsql
RUN dnf install -y php-pdo
RUN dnf install -y unzip

# php-fpmの設定変更
COPY php-fpm.d/www.conf /etc/php-fpm.d/www.conf
# Socketファイルの置き場所のディレクトリがデフォルトだと存在しないので作成する
RUN mkdir -p /var/run/php-fpm

# todo:プロジェクトに合わせて適宜調整
COPY --from=composer:2.1.6 /usr/bin/composer /usr/bin/composer

# php.iniの設定ファイルコピー
COPY php.ini /etc/php.d/50-docker.ini

# Apacheを入れる
RUN dnf install -y httpd
RUN apachectl -v
RUN systemctl enable httpd.service
# RUN apachectl start

EXPOSE 80

ENV APACHE_DOCUMENT_ROOT /path/to/new/root

CMD ["/usr/sbin/httpd", "-DFOREGROUND"]