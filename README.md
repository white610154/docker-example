### 新增網頁伺服器port

* 新增docker內port對應

```
// docker-compose.yml
    ports:
      - 8080:8080
      - [2906]:[6092] # 對外port(2906)對應docker內部port(6092)
```

* 新增該網頁伺服器資料夾

create folder inside nginx folder

```
|-- nginx
  |-- [myweb]
```

add volume connection config

```
// docker-compose-nginx.yml
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./nginx/[myweb]:/app/[myweb]
```

* 設定nginx(複製設定並更改兩處)

```
# nginx/nginx.conf
    server {
        listen 8080;

        root /app/admin;
        index index.html index.htm index.php;

        location / {
            try_files $uri $uri/ /index.html =404;
        }
    }
    server {
        listen [6092]; # docker內部port

        root /app;
        index index.html index.htm index.php;

        # 新增location - 網頁伺服器使用
        location / {
            try_files $uri $uri/ /index.html =404;
        }
        
        # 新增location - 檔案伺服器使用
        location ^/[myweb]/(.*)$ { # 網頁資料夾名稱
            try_files $uri $uri/ /index.html =404;
        }
    }
```
