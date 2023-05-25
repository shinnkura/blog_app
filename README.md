## プロジェクトのはじめ方

1 `docker compose up -d`

2 `docker compose ps`

3 `docker compose exec app bash`

4 `composer create-project --prefer-dist laravel/laravel laravel-project "10.*"`

5 `cd laravel-project`

6 `chown www-data storage/ -R`

5 `ブラウザでwelcomeページ表示 -> localhost:8000`



## 「.env」と「compose.yml」の設定を一致させる

▶︎元々

DB_CONNECTION=mysql

DB_HOST=127.0.0.1

DB_PORT=3306

DB_DATABASE=laravel

DB_USERNAME=root

DB_PASSWORD=


▶︎編集後

DB_CONNECTION=mysql

DB_HOST=db

DB_PORT=3306

DB_DATABASE=database

DB_USERNAME=db-user

DB_PASSWORD=db-pass
