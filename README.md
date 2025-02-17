# プロジェクト概要

このプロジェクトは、Docker Composeを使用してWordPress、MySQL、およびphpMyAdminをセットアップします。

## Docker Compose設定

### サービス: db (MySQL)
- **イメージ**: `mysql:5.7`
- **ボリューム**: `db_data:/var/lib/mysql` (データ永続化のため)
- **環境変数**:
  - `MYSQL_ROOT_PASSWORD`: `mywordpress`
  - `MYSQL_DATABASE`: `wordpress`
  - `MYSQL_USER`: `wordpress`
  - `MYSQL_PASSWORD`: `wordpress`

### サービス: wordpress
- **依存関係**: `db`
- **イメージ**: `wordpress:latest`
- **ボリューム**: `./wp:/var/www/html` (WordPressファイルの永続化)
- **ポート**: `8000:80` (アクセスポート)
- **環境変数**:
  - `WORDPRESS_DB_HOST`: `db:3306`
  - `WORDPRESS_DB_NAME`: `wordpress`
  - `WORDPRESS_DB_USER`: `wordpress`
  - `WORDPRESS_DB_PASSWORD`: `wordpress`

### サービス: phpmyadmin
- **イメージ**: `phpmyadmin:latest`
- **ボリューム**: `./phpmyadmin/phpmyadmin-misc.ini:/usr/local/etc/php/conf.d/phpmyadmin-misc.ini` (設定ファイル)
- **依存関係**: `db`
- **ポート**: `8888:80` (アクセスポート)

## 設定ファイル: phpmyadmin-misc.ini
- **設定**:
  - `allow_url_fopen`: `Off`
  - `max_execution_time`: `600`
  - `memory_limit`: `64M`
  - `post_max_size`: `4000M`
  - `upload_max_filesize`: `4000M`

## .htaccess設定
- **PHP設定**:
  - `max_execution_time`: `600`
  - `max_input_time`: `600`
  - `memory_limit`: `2000M`
  - `upload_max_filesize`: `2000M`
  - `post_max_size`: `2000M`

## 使用方法
1. DockerとDocker Composeがインストールされていることを確認してください。
2. プロジェクトディレクトリで以下のコマンドを実行します:
   ```
   docker-compose up -d
   ```
3. ブラウザで `http://localhost:8000` にアクセスしてWordPressを設定します。
4. phpMyAdminは `http://localhost:8888` でアクセス可能です。