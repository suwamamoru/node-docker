# 開発環境の立ち上げ
## 1. ディレクトリ 構成
```
root
└ docker
   └ db
└ myapp
└ docker-compose.yml
```
## 2. docker-compose.yml 作成
```
version: '3'
 
services:
  app:
    image: node:12
    environment:
      - TZ=Asia/Tokyo
      - DEBUG=app:*
    tty: true
    ports:
      - '3000:3000'
    volumes:
      - ./myapp:/app
    working_dir: /app
```
## 3. express 雛形作成
(1) express-generatorのインストール
```
$ npm install express-generator -g
```
(2) expressの実行
```
$ express --view=ejs myapp
```
(3)依存関係のインストール
```
$ cd myapp
$ npm install
```
## 4. 起動コマンドとDBをdocker-compose.ymlに追加
```
version: '3'
 
services:
  db:
    image: mysql:5.7
    container_name: mysqldb2
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: database
      MYSQL_USER: docker
      MYSQL_PASSWORD: docker
      TZ: 'Asia/Tokyo'
    command: mysqld --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
    volumes:
    - ./docker/db/data:/var/lib/mysql
    - ./docker/db/my.cnf:/etc/mysql/conf.d/my.cnf
    - ./docker/db/sql:/docker-entrypoint-initdb.d
    ports:
    - 3306:3306

  app:
    image: node:12
    environment:
      - TZ=Asia/Tokyo
      - DEBUG=app:*
    tty: true
    ports:
      - '3000:3000'
    volumes:
      - ./myapp:/app
    working_dir: /app
    command: >
      bash -c 'npm install &&
      npm start'
```
