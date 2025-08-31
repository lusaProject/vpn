bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)

{
  "inbounds": [
    {
      "port": 10000, 
      "protocol": "vmess",    
      "settings": {
        "clients": [
          {
            "id": "faef4e76-6e0e-4d6c-aea0-f8dfaf9456d9", 
            "alterId": 0 

          }
        ]
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
        # Show real IP in v2ray access.log
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }

{
    "inbounds": [
        {
            "port": 10000,
            "listen": "127.0.0.1",
            "protocol": "vmess",
            "settings": {
                "clients": [
                    {
                        "id": "faef4e76-6e0e-4d6c-aea0-f8dfaf9456d9",
                        "alterId": 0
                    }
                ]
            },
            "streamSettings": {
                "network": "ws",
                "wsSettings": {
                    "path": "/login"
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
  
