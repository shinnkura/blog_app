## 環境変数の設定

.env.example をコピーし、.env を同ディレクトリに作成してください。

**.env.exapmle**

```shl
WEB_PORT=80
DB_PORT=5432

DB_NAME=
DB_USER=
DB_PASSWORD=
```

**.env**

```shl
WEB_PORT=80
DB_PORT=5432

DB_NAME=mydb
DB_USER=yuto
DB_PASSWORD=pass
```

## Laravel のインストール

以下のコマンドで、Docker を立ち上げ、Laravel をインストールしてください。

```shl
docker-compose up -d
docker-compose exec app composer create-project --prefer-dist "laravel/laravel=10.*" .
```

## DB との接続確認

app コンテナと db コンテナとの接続を確認する

```shl
docker-compose exec app php artisan migrate
```

## Vue の環境構築

Vue を動かすために必要なパッケージをインストールします

```shl
docker-compose exec app npm install
docker-compose exec app npm install --save-dev vue @vitejs/plugin-vue
```

## Vite の設定変更

src/vite.config.js に追記

```shl
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';

<span style="color: green;">+ import vue from '@vitejs/plugin-vue';</span>


export default defineConfig({
    plugins: [
        laravel({
            input: ['resources/css/app.css', 'resources/js/app.js'],
            refresh: true,
        }),

+       vue(),

    ],

+   server: {

+       host: true,

+   },

});

```

## Vue がハンドルされていることを確認

src/resources/views/welcome.blade.php を、下記のように変更します。

```html
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />

    <title>Laravel Vue</title>
    @vite(['resources/css/app.css', 'resources/js/app.js'])
  </head>
  <body>
    <div id="app"></div>
  </body>
</html>
```

src/recouces/js/app.js を、下記のように変更します。

```js
import { createApp } from "vue";

import App from "./App.vue";

const app = createApp(App);

app.mount("#app");
```

src/resources/js/App.vue を、作成します。

```vue
<script setup>
import { ref } from "vue";

const message = ref("Hello World!");
</script>

<template>
  <h1>{{ message }}</h1>
</template>
```

Vite の開発用サーバーを立ち上げる

```shl
docker-compose exec app npm run dev
```

## ブラウザ上で確認

**localhost:80**にアクセスし、App.vue の内容が表示されていることを確認してください。

「Hello World！」という文字が表示されれば、正しくバンドルされています。

お疲れさまでした。
