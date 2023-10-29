---
title: AZ-700 Notes - Virtual WAN
date: 2023-10-29
tags:
- Azure
- Study
---

# Chapter 11 Virtual WAN

**Easy Mode Hub-Spoke**

* incorporates multiple hybrid connections (vpn/expressroute)
* incorporates multiple VNets
* Dynamically handles routing
* easily deploys NVA/Firewall

## Use case

You technically don't need to use it for anything, you can do the same with a normal hub-spoke built using VNets.

However, when you do, you're in charge of:

* add/update routes
* network gateway connections
* integration with virtual appliances / firewalls

Virtual WAN makes these tasks easier.

## Components

Virtual Wan

* overlay of entire WAN infrastructure
* includes 1 or more hubs

Hub

* managed VNet
* core of v wan
* main connection point of vnets, vpn, er circuits

Hybrid connections: s2s vpn, p2s vpn, er circuits

Other connections

* VNet connections
* hub-to-hub

## Transitive networking

In VWAN transitive is possible. 

VNets can communicate with other VNEts through hubs with standard WAN SKU. Not the basic sku.

Has managed router that forwards traffic between spokes.

Transitive across hubs in the same WAN.

Normally you have to explicitly create all the peerings, but in the WAN all VNets can communicate with eachother by default.

## SKUS

Basic: only s2s vpn. Not transitive.

Standard: all the rest
Transitive natworking, NVA in WAN

## Virtual Hub Routing

Virtual hubs contain their own route tables.

Hub connections (all types) associate and propagate routes to the hub's Default route table.

Static (custom) routes can be added.

Each network connected to a hub can either associate or propagate its routes.

### Association

The connection and its network can learn the routes and adopt those routes for its own connection.

An association is learning the virtual hub's routes and then taking on those routes for its own network routing table.

All connections associate to the Default route table. Propagation to the Default route table is optional to enable further isolation.

All connections will learn the hub's default routes, but not all connections will contribute to the Default route table.

### Propagation

A connection, or more specifically, the network behind its connection, adds its own network routing table to the hub's route table.

It is providing or propagating routes to the hub.

There is a Default and None route table. Propagating to the None route table does not add VNet routes.

This isolates the the network because its routes are not published to the hub.

### Custom Route Tables

Additional route tables alongside the hub's Default table.

Choose which connections associate (learn) and propagate (add to) custom route tables.

Useful for:

* isolating groups of networks from other networks
* routing traffic through several NVA's attached to the hub

**Custom route tables will take priority over custom route tables**

## Virtual WAN Hybrid Connections

Merging the WAN with on-site networks through S2S/P2S/VPN and ExpressRoute.

### Scale Units

Important for exam. 

Think of these as a "bandwith SKU".

Assigns the aggregate througput (bandwith) to the connection unit. Expressed in Mbps or Gbps.

VPN and ExpressRoute connections use different scale units.

## Links:

202310290910

[[az-700]]

