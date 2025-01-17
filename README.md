# Wireguard-Peer-Manager
![image](https://user-images.githubusercontent.com/4666566/117325184-56f7f800-ae45-11eb-9003-b85aadbf5ff0.png)
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fhouse-of-vanity%2FWireguard-Peer-Manager.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Fhouse-of-vanity%2FWireguard-Peer-Manager?ref=badge_shield)

Adds Wireguard peers to config, reload it and send client config back via Telegram. 

**FYI: That tool stores client private keys into server config as comments.**

How to use:

```shell
# create initial wg config or use your own.
# P.S. Keep in mind that WPM can't manage peers created my hands
# due to absence of client private key.
$ cd /etc/wireguard && mkdir clients
$ cat > wg0.conf <<EOF
[Interface]
Address = 10.150.200.1/24
ListenPort = 51820
PrivateKey = $(wg genkey)
PostUp = iptables -A FORWARD -i %i -o %i -j ACCEPT
PostDown = iptables -D FORWARD -i %i -o %i -j ACCEPT
SaveConfig = false
EOF

# install python and system requirements.
$ pip3 install -r requirements.txt
$ apt install qrencode

# Create config. It's optionally.
$ cp wpm_example.conf wpm.conf

# CLI usage. Client configs saved into `clients/peer_name.{conf,-qr.png,-qr.txt}`
$ python3 gen.py --peer my-pc   # add a new peer `my-pc`
$ python3 gen.py --delete my-pc # delete peer `my-pc`
$ python3 gen.py --update       # just regenerate all configs in `clients/`

# Telegram bot usage
$ TG_TOKEN=1292121488:AAG... TG_ADMIN=<your_username> python3 bot.py
```

## Config
Key | Default | Description
------------ | ------------- | ------------
allowed_ips | 0.0.0.0 | allowed_ips for generated peer configs.
dns | 8.8.8.8 | DNS for peer configs
hostname | $(hostname -f) | server address for peer configs. May be an IP.
config | wg0 | WireGuard config to work with. 


## Telegram Interface

<img src="https://user-images.githubusercontent.com/4666566/117370133-cc31f000-ae7a-11eb-93fd-a390d2616da8.png" alt="drawing" width="450"/> <img src="https://user-images.githubusercontent.com/4666566/117377076-48323500-ae87-11eb-9602-a0cd3072ff53.png" alt="drawing" width="350"/>




## License
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fhouse-of-vanity%2FWireguard-Peer-Manager.svg?type=large)](https://app.fossa.com/projects/git%2Bgithub.com%2Fhouse-of-vanity%2FWireguard-Peer-Manager?ref=badge_large)