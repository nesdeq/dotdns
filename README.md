# dotdns

I created a "portable" DNS over TLS + DNS Filtering Setup that runs on macos, locally, to feel safe from a DNS perspective when on the go.

This setup should work on all OSs, however you might need to apply a different setup regarding loopback interfaces (see below for macos).

Pi-hole + Unbound on macOS. DNS-over-TLS to Quad9.

## Setup

Set your own pihole web-ui password in .env.

```bash
cp .env.example .env
vim .env
docker compose up -d
```

Add another loopback interface (.1:53 is taken by macos) and set DNS to this interface for all WiFis on boot.

Root crontab (`sudo crontab -e`):
```
@reboot /sbin/ifconfig lo0 alias 127.0.0.2
@reboot /usr/sbin/networksetup -setdnsservers Wi-Fi 127.0.0.2
```

Web UI: http://127.0.0.2/admin

Add Filter Lists to your liking. I like https://firebog.net/ as a source.

Caveat: After a fresh boot/reboot (who does that anyway?) it will take a minute until the OS feels "online".