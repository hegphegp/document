## openresty实战

### 1.0 配置参数说明

#### 1.1 proxy_pass后面接URL或者路径， 末尾加与不加/(斜线)的区别

* <strong>加/</strong> 斜线的情况，绝大多数都是用这种方式的配置  
```
location /pss/ {
    proxy_pass http://ip:port/;
}
# 假设外界访问nginx的路径是 /pss/bill.html
# 被代理的真实访问路径为：http://ip:port/bill.html，即在nginx机器访问，该 http://ip:port/bill.html 要存在
```

* <strong>不加/</strong> 斜线的情况

```
location /pss/ {
    proxy_pass http://ip:port;
}
# 假设外界访问nginx的路径是 /pss/bill.html
# 被代理的真实访问路径为：http://ip:port/pss/bill.html，即在nginx机器访问，该 http://ip:port/pss/bill.html 要存在

# nginx官方例子的首页配置
# server {
#     listen       80;
#     server_name  localhost;

#     location / {
#         root   /usr/local/openresty/nginx/html;
#         index  index.html index.htm;
#     }
# }
```

```
mkdir -p openresty-1.15.8.1-1-alpine && cd openresty-1.15.8.1-1-alpine
echo '''''

openresty:
  image: openresty/openresty:1.15.8.1-1-alpine
  restart: always
  ports:
    - '80:80'
  links:
    - aaa-domain
    - bbb-domain
    - ccc-domain

aaa-domain:
  image: openresty/openresty:1.15.8.1-1-alpine
  command: sh -c 'echo "<h1>aaaaaaaaa.domain</h1>" > /usr/share/nginx/html/index.html && nginx -g "daemon off;"'
  ports:
    - '8081:80'

bbb-domain:
  image: openresty/openresty:1.15.8.1-1-alpine
  command: sh -c 'echo "<h1>bbbbbbbbb.domain</h1>" > /usr/share/nginx/html/index.html && nginx -g "daemon off;"'
  ports:
    - '8082:80'

ccc-domain:
  image: openresty/openresty:1.15.8.1-1-alpine
  command: sh -c 'echo "<h1>ccccccccc.domain</h1>" > /usr/share/nginx/html/index.html && nginx -g "daemon off;"'
  ports:
    - '8083:80'

''''' > docker-compose.yml
```