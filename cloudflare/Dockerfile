FROM nginx:latest
WORKDIR /usr/share/nginx/html
RUN curl -L https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-arm -o cloudflared-linux-arm
RUN chmod 755 cloudflared-linux-arm
RUN mv cloudflared-linux-arm /usr/local/bin/cloudflared
# RUN cloudflared service install

# cloudflared tunnel --url localhost:80
# docker run --rm -it  -p 80:80/tcp clouddemo:latest

# cloudflared tunnel login
# cloudflared tunnel create {tunnel_name}
# cloudflared tunnel route dns [{UUID} or {NAME}] sub.domain.com
# cloudflared tunnel --config config.yaml run {tunnel_name}

#####################
# config.yaml
#####################
# tunnel: {UUID}
# credentials-file: /home/pi/.cloudflared/{UUID}.json
#
# ingress:
#   - hostname: sub.domain.com
#     service: http://localhost:80
#   - service: http_status:404