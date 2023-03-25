# README

## Early Hints
### 目的
- アセットのロード開始タイミングを早める

### 試したけど上手くいかなかった

```
# /usr/local/etc/h2o/h2o.conf
# H2Oの設定(localhostの9090番ポートに届いたリクエストを3000番ポートへプロキシする)
hosts:
  localhost:
    listen:
      port: 9090
      ssl:
        certificate-file: /usr/local/etc/h2o/localhost.crt
        key-file: /usr/local/etc/h2o/localhost.key
    paths:
      /:
        proxy.reverse.url: http://127.0.0.1:3000/
        proxy.preserve-host: ON
access-log: /usr/local/var/h2o/access-log
error-log: /usr/local/var/h2o/error-log
```

```shell
# 自己証明書の生成
% openssl req -nodes -x509 -new -days 36500 \
  -subj "/CN=localhost" \
  -keyout /usr/local/etc/h2o/localhost.key \
  -out /usr/local/etc/h2o/localhost.crt

#=>
Generating a 2048 bit RSA private key
.......+++
..................+++
writing new private key to '/usr/local/etc/h2o/localhost.key'
-----
```

```
# 起動がうまくいかなかった
% h2o -c /usr/local/etc/h2o/h2o.conf

#=>
[INFO] raised RLIMIT_NOFILE to -1
fetch-ocsp-response (using LibreSSL 2.8.3)
failed to extract ocsp URI from /usr/local/etc/h2o/localhost.crt
```

```
# これしても3000番ポートで立ち上がるだけ
% bin/rails s --early-hints
```

### CSP(Content Security Policy)

### nonce, strict-dynamic, XSS対策
- https://inside.pixiv.blog/kobo/5137

### H2O
- https://github.com/h2o/h2o/
- https://formulae.brew.sh/formula/h2o
- https://h2o.examp1e.net/

