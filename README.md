## Scope
* This is based on the documentetion here: [Zero trust](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-guide/local/#set-up-a-tunnel-locally-cli-setup)
* I am using this fictional domain: domain.com
* I will call this tunnel **lab**
* I will configure the next CNAME:
  1. portainer.domain.com
  2. nextcloud.domain.com
  3. pihole.domain.com
  4. filezilla.domain.com
  5. domain.com

## Install and setup Cloudflared
```
wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb && dpkg -i cloudflared-linux-amd64.deb
cloudflared tunnel login
cloudflared tunnel create lab
```
See the generated tunnel ID:
```ls /root/.cloudflared```
For example: `xxxx-xxxx-xxxx-xxxx-xxxx.json`

## Create a `config.yml` file:
`nano /root/.cloudflared/config.yml`
```
tunnel: xxxx-xxxx-xxxx-xxxx-xxxx
credentials-file: /root/.cloudflared/xxxx-xxxx-xxxx-xxxx-xxxx.json
ingress:
  - hostname: portainer.shifu.fun
    service: https://localhost:9443
    originRequest:
      noTLSVerify: true
  - hostname: nextcloud.shifu.fun
    service: http://localhost:8888
    disableChunkedEncoding: true
    noHappyEyeballs: true
  - hostname: pihole.shifu.fun
    service: http://localhost:80
    disableChunkedEncoding: true
    noHappyEyeballs: true
  - hostname: filezilla.shifu.fun
    service: http://localhost:3000
    disableChunkedEncoding: true
    noHappyEyeballs: true
  - hostname: shifu.fun
    service: http://localhost:8080
    disableChunkedEncoding: true
    noHappyEyeballs: true
  - service: http_status:404
```
Now, run the tunnel:
```cloudflared tunnel run lab```
To make it a service:
```cloudflared service install```
To remove the service:
```
systemctl stop cloudflared
rm -r /etc/systemd/system/cloudflared*
rm -r /etc/cloudflared/
```
