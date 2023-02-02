* This is based on the documentetion here: [Zero trust](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-guide/local/#set-up-a-tunnel-locally-cli-setup)
* Buy a domain, example: domain.com
* We will call this tunnel **lab**
* I will configure the next CNAME:
> portainer.domain.com
> nextcloud.domain.com
> pihole.domain.com
> domain.com > for wordpress

## Install and setup Cloudflared
```
wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb && dpkg -i cloudflared-linux-amd64.deb
cloudflared tunnel login
cloudflared tunnel create lab

```
