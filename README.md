Nginx 配置示例

1. 安装 V2Ray
```bash
https://github.com/v2fly/fhs-install-v2ray
```

2. V2Ray 配置（WebSocket 方式）

文件路径：`/usr/local/etc/v2ray/config.json`

```json
{
  "inbounds": [
    {
      "port": 1111,
      "listen": "127.0.0.1",
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "0000-0000-0000-0000",
            "alterId": 0
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
          "path": "/xxx"
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```

3. Nginx 配置

文件路径：`/etc/nginx/conf.d/v2ray.conf`

```nginx
location /login {
    if ($http_upgrade != "websocket") {
        return 404;
    }
    proxy_redirect off;
    proxy_pass http://127.0.0.1:10000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```

4. 加速服务

```bash
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
lsmod | grep bbr    出现 tcp_bbr 加速成功
```

5. 客户端软件

```bash
https://www.linuxv2ray.com/client/
```

