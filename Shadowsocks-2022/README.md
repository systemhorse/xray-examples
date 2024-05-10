# Shadowsocks 2022

Install `xray` on server with https://github.com/XTLS/Xray-install.

Generate password:

```
openssl rand -base64 32
```

Edit `/usr/local/etc/xray/config.json`:

```
{
    "log": {
        "loglevel": "warning"
    },
    "routing": {
        "domainStrategy": "AsIs",
        "rules": [
            {
                "type": "field",
                "ip": [
                    "geoip:private"
                ],
                "outboundTag": "block"
            }
        ]
    },
    "inbounds": [
        {
            "listen": "0.0.0.0",
            "port": 1234,
            "protocol": "shadowsocks",
            "settings": {
                "method": "2022-blake3-aes-256-gcm",
                "password": "{{ password }}"
            },
            "streamSettings": {
                "network": "tcp"
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "freedom",
            "tag": "direct"
        },
        {
            "protocol": "blackhole",
            "tag": "block"
        }
    ]
}
```

Restart `xray`:

```
systemctl restart xray
```

GUI clients as per README for https://github.com/XTLS/Xray-core.
