# ユーザー作成

```sh
docker compose exec dolt bash
dolt sql
CREATE USER 'y_mayo_dev'@'%' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'y_mayo_dev'@'%';
FLUSH PRIVILEGES;
```

# clone

```sh
docker compose exec script bash
export DOLT_REMOTE_PASSWORD=password
dolt clone --user y_mayo_dev http://dolt:8000/getting_started getting_started
```

# fetch

何故か毎回userを指定しないといけない。

```sh
docker compose exec script bash
export DOLT_REMOTE_PASSWORD=password
dolt fetch --user y_mayo_dev
```

# 直接リモートのDBにアクセス

```sh
docker compose exec script bash
dolt config --global --add user.email y_mayo_dev@gmail.com
dolt config --global --add user.name y_mayo_dev
dolt --host dolt --no-tls --user y_mayo_dev sql
```
