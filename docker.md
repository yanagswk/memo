## コンテナ作成
`docker (container) run --name test1 -d -p 8080:80 -v /mnt/c/Users/yanag/Documents/code/github/memo/php/htdocs:/var/www/html php:8.0.3-apache`  

|  オプション  | 意味   |
| ---- | ---- |
|  --name  |  コンテナの名前  |
|  -d  |  デタッチモード  |
|  --rm  |  コンテナの終了時に削除。関連するボリュームも削除  |
|  -v  |  ボリューム設定  |
|  -p  |  ポート設定  |  

- -vのボリューム設定は、絶対パスをスラッシュで指定しなければならない。windowsの場合は、Cドライブから。  
上記の場合は、wslを使用しているため、mntからになっている。
- containerコマンドからコンテナにアクセス  
`docker exec -it test1 /bin/bash`  
`docker container cp test1:/usr/local/etc/php/php.ini-development ./`(ボリューム操作)

## Dockerfile作成  
- オリジナルのイメージを作ることができる。  
    ```Dockerfile
    FROM php:8.0.3-apache
    COPY php.ini /usr/local/etc/php/
    ```
- イメージを作ったら、イメージを作成する。  
    `docker build -t イメージ名:タグ名 Dockerfileのディレクトリ`

- 作成したイメージをもとにコンテナ作成  
    `docker (container) run --name test1 -d -p 8080:80 -v /mnt/c/Users/yanag/Documents/code/github/memo/php/htdocs:/var/www/html イメージ名`
