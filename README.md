# Project tree
```bash
.
├── README.md 
├── cloudflare ☁️
│   └── Dockerfile 🐳
├── docker-compose.yaml
├── tunnel-config 🛠
│   ├── cert.pem 🔑
│   ├── ae770dec-ca0b-469d-b58b-30b8c5ec8069.json
│   └── config.yaml ⚙️
└── www 🌎
    ├── 50x.html 💣
    └── index.html 
```

---
## cloudflare tunnel command
รัน tunnel โดยที่ยังไม่ได้ลงทะเบียนกับ Cloudflare จะคล้ายกับการรัน ngrok
```bash
root@local:~ $ cloudflared tunnel --url localhost:80
```
---
รัน tunnel โดยที่ลงทะเบียนกับ Cloudflare แล้ว
```bash
# login เพื่อสร้างไฟล์ cert.pem
root@local:~ $ cloudflared tunnel login
```
เมื่อทำการ authenticate กับ cloudflared เรียบร้อยแล้ว จะมีข้อความประมาณนี้
```
You have successfully logged in.
If you wish to copy your credentials to a server, they have been saved to:
/root/.cloudflared/cert.pem
```
```bash
# create tunnel
root@local:~ $ cloudflared tunnel create {tunnel_name}
# หลังจากสร้าง tunnel แล้วให้สร้างไฟล์ config.yaml

# เราสามารถลองเรียกดู tunnel ที่สร้างโดยใช้คำสั่ง tunnel list
root@local:~ $ cloudflared tunnel list

# setup dns
root@local:~ $ cloudflared tunnel route dns [{UUID} or {NAME}] sub.domain.com

# setup http
root@local:~ $ cloudflared tunnel --config config.yaml run {tunnel_name}
```

---
## [config.yaml](tunnel-config/config.yaml)
```yaml
# tunnel: {{UUID}}
tunnel: ae770dec-ca0b-469d-b58b-30b8c5ec8069
# credentials-file: /home/pi/.cloudflared/{UUID}.json
credentials-file: /etc/cloudflared/ae770dec-ca0b-469d-b58b-30b8c5ec8069.json

ingress:
  # Rules map traffic from a hostname to a local service:
  # - hostname: domain.com
  #   service: http://localhost:80
  # - hostname: www.domain.com
  #   service: http://localhost:80
  # Rules can match the request's hostname to a wildcard character:
  # - hostname: '*.domain.com'
  #   service: http://localhost:80
  - hostname: sub.domain.com
    service: http://localhost:80
  # An example of a catch-all rule:
  # - service: https://localhost:80
  # Example of a rule responding to traffic with an HTTP status:
  - service: http_status:404
#
#
# Ingress rules:
# https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/configuration/local-management/ingress/
```