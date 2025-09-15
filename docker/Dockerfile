FROM php:8.4.12-apache

# 必要なパッケージをインストール
RUN apt-get update && apt-get install -y \
    curl \
    git \
    unzip \
    && rm -rf /var/lib/apt/lists/*

# Node.js 24.x をインストール（NodeSourceリポジトリを使用）
RUN curl -fsSL https://deb.nodesource.com/setup_24.x | bash - && \
    apt-get install -y nodejs && \
    rm -rf /var/lib/apt/lists/*

# www-dataユーザーのUIDを1001に変更(Amazon Linux 2023に揃える）
RUN usermod -u 1001 www-data && \
    groupmod -g 1001 www-data

# 作業ディレクトリを設定
WORKDIR /var/www/laravel

# Apacheの設定ファイルを上書き
copy ./000-default.conf /etc/apache2/sites-available/000-default.conf

# mod_rewriteを有効化（Laravelで必要）
RUN a2enmod rewrite

# ディレクトリの所有者をwww-dataに変更
RUN chown -R www-data:www-data /var/www

# npmのキャッシュディレクトリをwww-dataが書き込めるように設定
RUN mkdir -p /var/www/.npm && \
    chown -R www-data:www-data /var/www/.npm

# Composerをインストール
COPY --from=public.ecr.aws/docker/library/composer:2.8 /usr/bin/composer /usr/local/bin/composer

# www-dataユーザーとして実行
USER www-data