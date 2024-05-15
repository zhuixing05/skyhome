{
  "dns": {
    "hosts": {
      "domain:googleapis.cn": "googleapis.com"
    },
    "servers": [
      "1.1.1.1",
      {
        "address": "223.5.5.5",
        "domains": [
          "geosite:cn",
          "geosite:geolocation-cn"
        ],
        "expectIPs": [
          "geoip:cn"
        ],
        "port": 53
      }
    ]
  },
  "fakedns": [
    {
      "ipPool": "198.18.0.0/15",
      "poolSize": 10000
    }
  ],
  "inbounds": [
    {
      "port": 10808,
      "protocol": "socks",
      "settings": {
        "auth": "noauth",
        "udp": true,
        "userLevel": 8
      },
      "sniffing": {
        "destOverride": [
          "http",
          "tls"
        ],
        "enabled": true
      },
      "tag": "socks"
    },
    {
      "port": 10809,
      "protocol": "http",
      "settings": {
        "userLevel": 8
      },
      "tag": "http"
    },
    {
      "listen": "127.0.0.1",
      "port": 10853,
      "protocol": "dokodemo-door",
      "settings": {
        "address": "1.1.1.1",
        "network": "tcp,udp",
        "port": 53
      },
      "tag": "dns-in"
    }
  ],
  "log": {
    "loglevel": "warning"
  },
  "outbounds": [
    {
      "mux": {
        "concurrency": -1,
        "enabled": false,
        "xudpConcurrency": 8,
        "xudpProxyUDP443": ""
      },
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "zfa01.333210.xyz",
            "port": 40382,
            "users": [
              {
                "alterId": 0,
                "encryption": "",
                "flow": "",
                "id": "8630e1bc-d987-4815-b705-04c04f5eb7a7",
                "level": 8,
                "security": "auto"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "tcp",
        "security": "tls",
        "tcpSettings": {
          "header": {
            "type": "none"
          }
        },
        "tlsSettings": {
          "allowInsecure": false,
          "fingerprint": "",
          "serverName": "400567.xyz",
          "show": false
        }
      },
      "tag": "proxy"
    },
    {
      "protocol": "freedom",
      "settings": {
        "domainStrategy": "UseIP"
      },
      "tag": "direct"
    },
    {
      "protocol": "blackhole",
      "settings": {
        "response": {
          "type": "http"
        }
      },
      "tag": "block"
    },
    {
      "protocol": "dns",
      "tag": "dns-out"
    }
  ],
  "policy": {
    "levels": {
      "8": {
        "connIdle": 300,
        "downlinkOnly": 1,
        "handshake": 4,
        "uplinkOnly": 1
      }
    },
    "system": {
      "statsOutboundUplink": true,
      "statsOutboundDownlink": true
    }
  },
  "remarks": "L1|香港04|中转|流媒体|3x",
  "routing": {
    "domainStrategy": "IPIfNonMatch",
    "rules": [
      {
        "inboundTag": [
          "dns-in"
        ],
        "outboundTag": "dns-out"
      },
      {
        "ip": [
          "1.1.1.1"
        ],
        "outboundTag": "proxy",
        "port": "53"
      },
      {
        "ip": [
          "223.5.5.5"
        ],
        "outboundTag": "direct",
        "port": "53"
      },
      {
        "domain": [
          "domain:googleapis.cn"
        ],
        "outboundTag": "proxy"
      },
      {
        "ip": [
          "geoip:cn"
        ],
        "outboundTag": "direct"
      },
      {
        "domain": [
          "geosite:cn"
        ],
        "outboundTag": "direct"
      },
      {
        "domain": [
          "geosite:geolocation-cn"
        ],
        "outboundTag": "direct"
      },
      {
        "outboundTag": "proxy",
        "port": "0-65535"
      }
    ]
  },
  "stats": {}
}