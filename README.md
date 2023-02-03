# wordpress_docker


- 適当なディレクトリを作りその中に、docker-compose.ymlファイルを作る。（作ったディレクトリをwordpressとして以下説明）

- wordpressディレクトリにhtmlディレクトリを作る。

- /var/www/htmlディレクトリを作る。

- docker-compose.ymlを作成

```
version: "3.7"
services:
  db:
    image: mysql:5.7
    container_name: wp_mysql
    #restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root # rootユーザのパスワード（任意に）
      MYSQL_DATABASE: wp_db # WordPress用データベース名（任意に）
      MYSQL_USER: wp_user # WordPress用データベース接続ユーザ名（任意に）
      MYSQL_PASSWORD: root # WordPress用データベース接続パスワード（任意に）
  WordPress:
    image: wordpress:latest
    container_name: wp
    #restart: always
    depends_on:
      - db
    ports:
      - "10090:80"
    environment:
      WORDPRESS_DB_HOST: db:3306 # DBサーバ名：ポート番号
      WORDPRESS_DB_USER: wp_user # WordPress用データベース接続ユーザ名(dbに合わせる)
      WORDPRESS_DB_PASSWORD: root # WordPress用データベース接続パスワード(dbに合わせる)
      WORDPRESS_DB_NAME: wp_db # WordPress用データベース名(dbに合わせる)
      WORDPRESS_DEBUG : 1 # デバッグモードON=1/OFF=0
    volumes:
      - ./html:/var/www/html # 事前に作っておく
  phpmyadmin:
    image: phpmyadmin/phpmyadmin:latest
    container_name: phpmyadmin_ingrid
    #restart: always
    depends_on:
      - db
    ports:
      - 10099:80
```

wordpressディレクトリで以下のコマンド実行するとMySQL、WordPress、MySQLAdminが動く３つのコンテナーが起動する。

```
docker-compose up -d
```

上記コンテナを終了する場合は以下のコマンドを実行する。

```
docker-compose down --volumes
```

注） 実行環境はUbuntu 22.04LTS