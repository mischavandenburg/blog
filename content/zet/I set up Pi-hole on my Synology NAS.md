---
title: I set up Pi-hole on my Synology NAS
date: 2024-08-11
tags:
  - Homelab
  - Privacy
  - Security
---

Transition to manifest V3 is now starting and Ublock Origin might not be supported in Chrome in the future.

Because of my transition from a mouse to a pen tablet, I've already been experimenting with Qutebrowser for the past couple of weeks and I really enjoy it. It feels much closer to vim because I can run commands like this: ":set darkmode" which feels familiar and I can search my history very easily.

Though it uses elements from Brave's ad blocker, the QtWebEngine is based on Chromium so I expect this will be affected by the manifest V3 rollout as well. Which means that I need another way to block ads.

So, I'm setting up PiHole to add another layer of ad blocking to the setup and to give a hand to Qutebrowser.

As I have a YouTube premium subscription I'm already enjoying YouTube without ads. I happily pay for it because I also use it for YouTube music and it's a way to support musicians and creators without having to suffer ads.

But I can't stand ads while I'm reading blog posts.

I'm happy to report that I now have PiHole running and all my traffic is routed through it. Having a Synology and Unifi router made this a piece of cake.

I didn't want the Pi-Hole in my Kubernetes cluster so I'm just running it on my Synology in a docker container.

Then I set the DNS server for the VLAN to point towards my Synology and done!

Very interesting to scroll through the query logs and see what's being blocked.

At first, I had it set up like this:

[How to Run Pi-hole on a Synology NAS - Pi My Life Up](https://pimylifeup.com/pi-hole-synology-nas/)

```docker
version: "3"

services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:

      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp"
      - "3009:80/tcp"
    environment:
      TZ: 'CET'
      WEBPASSWORD: 'yum apple pie'
    volumes:
      - './etc-pihole:/etc/pihole'
      - './etc-dnsmasq.d:/etc/dnsmasq.d'
    cap_add:
      - NET_ADMIN
    restart: unless-stopped
```

## Using Macvlan

Later I wanted it available on a separate IP. This allowed me to view the individual clients in Pi-hole. In other words, I can see exactly which device the requests are coming from. When running it on the same IP as the Synology, I only saw the IP address of the internal docker network on the Synology.

[Pi-hole in Container Manager on a Synology NAS (drfrankenstein.co.uk)](https://drfrankenstein.co.uk/pi-hole-in-container-manager-on-a-synology-nas/)

```yaml
services:
  pihole-macvlan:
    image: pihole/pihole:latest
    container_name: pihole-macvlan
    cap_add:
      - CAP_NET_RAW
      - CAP_NET_BIND_SERVICE
      - CAP_CHOWN
    environment:
      - PIHOLE_UID=1028
      - PIHOLE_GID=65536
      - TZ=CET
      - WEBPASSWORD=letseatsomemorepie
      - DNSMASQ_LISTENING=all
      - WEB_PORT=3001
      - DNSMASQ_USER=pihole
      - FTLCONF_LOCAL_IPV4=192.168.120.10
    volumes:
      - "./etc-pihole:/etc/pihole"
      - "./etc-dnsmasq.d:/etc/dnsmasq.d"
    networks:
      macvlan:
        ipv4_address: 192.168.120.10
    restart: always

networks:
  macvlan:
    name: macvlan
    driver: macvlan
    driver_opts:
      parent: eth0
    ipam:
      config:
        - subnet: "192.168.120.0/24"
          ip_range: "192.168.120.254/24"
          gateway: "192.168.120.1"
```
