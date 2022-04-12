# 開発環境の立ち上げ
## 1. ディレクトリ 構成
ディレクトリを以下のように構成する。
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
