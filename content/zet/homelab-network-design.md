---
Title: Designed The Network For My Homelab
date: 2024-01-13
tags:
- homelab
- networking
- Excalidraw
---

My ISP router only allowed me to assign 200 IP addresses and it didn't have any possibility to create VLANS, so it was time for an upgrade to my setup.

Now that I'm hosting several clusters in my homelab and exposing things to the internet I can justify to spend the money on this hardware and the complexity of dividing up my home network into VLANS.

I bought a Unifi Express gateway and the Unifi Lite 8 PoE managed switch.

The gateway will function as the main portal to the internet and the WiFi access point. The switch will be located in my pantry where all the devices for my homelab are currently.

![](home-network-design.png.md)


# Naming Strategy

I'm also going to have some fun with the naming strategy. I love Nordic mythology so I picked that as a theme.

## Network Name:

- **Asgard**: A reference to the home of the gods in Norse mythology, symbolizing a powerful and central network.

### VLAN Naming Strategy:

1. **Production Kubernetes Cluster**: `Valhalla` - Named after the majestic hall where heroes reside, representing the strength and importance of the production environment.
2. **Staging Kubernetes Cluster**: `Midgard` - Midgard, the world of humans in Norse mythology, can symbolize a testing ground that's closer to the 'real world' scenarios.
3. **Main VLAN (Work & Personal Devices)**: `Yggdrasil` - Named after the great tree that connects all the worlds, symbolizing the connectivity and central role of this VLAN in the network.
4. **IoT VLAN**: `Niflheim` - The realm of fog and mist, a metaphor for the mysterious and ubiquitous nature of IoT devices.
5. **Guest Network VLAN**: `Jotunheim` - Inspired by the realm of giants in Norse mythology, this guest network embodies the vastness and mystery of Jotunheim. It's a separate, secure space for visitors, representing the outer layers of my realm.

![](/jotunheim.png)

## Why UniFi?

* a colleague of mine has it and is very satisfied with it
* saw a few videos of Techno Tim and was very impressed by his setup
* looks like a very easy way to manage the network
* provides insight into traffic flow
* beautiful dashboard and UI
* I like ecosystems with good documentation
* allows expanding within the same ecosystem

## Links:

202401130601
