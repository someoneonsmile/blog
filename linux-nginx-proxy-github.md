# nginx 反向代理加速 github

|    date    |  tag  |
|    ---     |  ---  |
| 2020/09/11 | nginx |

## nginx 配置如下

> 放置到 `nginx/sites-available/github` 中, 并软链接到 `nginx/sites-enable/github`

```nginx
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name github.closer.site;

    location / {
        proxy_hide_header referrer-policy;
        proxy_hide_header content-security-policy;
        proxy_hide_header Strict-Transport-Security;
        proxy_hide_header set-cookie;

        proxy_set_header Host github.com;
        proxy_set_header Accept-Encoding "";
        proxy_set_header Connection "";

        proxy_http_version 1.1;
        proxy_connect_timeout 10s;
        proxy_read_timeout 10s;
        proxy_socket_keepalive on;

        sub_filter "\"https://github.com" "\"https://github.closer.site";
        sub_filter "\"https://raw.githubusercontent.com" "\"https://raw-github.closer.site";
        sub_filter "\"https://github.githubassets.com" "\"https://assets-github.closer.site";
        sub_filter "\"https://codeload.github.com" "\"https://codeload-github.closer.site";
        sub_filter_once off;

        proxy_redirect https://raw.githubusercontent.com https://raw-github.closer.site;
        proxy_redirect https://github.githubassets.com https://assets-github.closer.site;
        proxy_redirect https://codeload.github.com https://codeload-github.closer.site;

        proxy_pass https://github.com;
    }
}


server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name raw-github.closer.site;

    location / {
        proxy_hide_header referrer-policy;
        proxy_hide_header content-security-policy;
        proxy_hide_header Strict-Transport-Security;
        proxy_hide_header set-cookie;

        proxy_set_header Host raw.githubusercontent.com;
        proxy_set_header Accept-Encoding "";
        proxy_set_header Connection "";

        proxy_http_version 1.1;
        proxy_connect_timeout 10s;
        proxy_read_timeout 10s;
        proxy_socket_keepalive on;

        proxy_pass https://raw.githubusercontent.com;
    }
}


server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name assets-github.closer.site;

    location / {
        proxy_hide_header referrer-policy;
        proxy_hide_header content-security-policy;
        proxy_hide_header Strict-Transport-Security;
        proxy_hide_header set-cookie;

        proxy_set_header Host github.githubassets.com;
        proxy_set_header Accept-Encoding "";
        proxy_set_header Connection "";

        proxy_http_version 1.1;
        proxy_connect_timeout 10s;
        proxy_read_timeout 10s;
        proxy_socket_keepalive on;

        proxy_pass https://github.githubassets.com;
    }
}


server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name codeload-github.closer.site;

    location / {
        proxy_hide_header referrer-policy;
        proxy_hide_header content-security-policy;
        proxy_hide_header Strict-Transport-Security;
        proxy_hide_header set-cookie;

        proxy_set_header Host codeload.github.com;
        proxy_set_header Accept-Encoding "";
        proxy_set_header Connection "";

        proxy_http_version 1.1;
        proxy_connect_timeout 10s;
        proxy_read_timeout 10s;
        proxy_socket_keepalive on;

        proxy_pass https://codeload.github.com;
    }
}
```
