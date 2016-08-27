# Purify

A lightweight DNS-based adblocker packaged as an 8MB Docker image. Blocked domains are updated daily from [Steven Black's hosts project](https://github.com/StevenBlack/hosts).

## Running

```
git clone https://github.com/schmich/purify && cd purify
docker build -t schmich/purify .
docker run -d -p 53:53/tcp -p 53:53/udp --cap-add=NET_ADMIN --restart always schmich/purify --log-facility=-
```

## Updating Clients

- DNS nameservers can be set on your router or on individual devices
- Phones must be rooted to set DNS for cellular networks
- Flush your device's DNS cache to notice immediate effects
  - macOS: `sudo dnscacheutil -flushcache; sudo killall -HUP mDNSResponder`
  - Windows: `ipconfig /flushdns`
  - Chrome: `chrome://net-internals/#dns`
  - Phones: Reboot

## Debugging

Run the server in the foreground and log all DNS queries:

```
docker run -p 53:53/tcp -p 53:53/udp --cap-add=NET_ADMIN --restart always schmich/purify --log-facility=- --log-queries
```

Run a DNS query directly against your server (where `1.2.3.4` is your server's IP):

```
dig @1.2.3.4 doubleclick.net
```

Blocked domains resolve to `0.0.0.0`.

## Credits

- Blocked domains: [Steven Black's hosts project](https://github.com/StevenBlack/hosts)
- dnsmasq base image: [Andy Shinn's docker-dnsmasq project](https://github.com/andyshinn/docker-dnsmasq)
