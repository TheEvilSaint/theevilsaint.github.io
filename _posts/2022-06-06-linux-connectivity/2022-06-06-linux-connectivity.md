---
layout: post
title: 'Linux Connectivity'
date: '2022-06-06'
description: 'A look at various commands that can be used for configuring Linux network connectivity.'
coverimage: Linux_Connectivity.jpg
tags: networking
published: true
posttype: article
categories: article
---
# Linux Connectivity

- `ifconfig` is part of the `net-tools` package.
- `ip` is an alternative command from `iproute2util` package.

View IP Addresses

```
ifconfig
ip a
```

## Adding or Deleting an IP Address

ifconfig

```
ifconfig eth0 add 192.168.80.174
ifconfig eth0 del 192.168.80.174
```

ip

```
ip a add 192.168.80.174 dev eth0
ip a del 192.168.80.174 dev eth0
```

static interface configurations (\etc\network\interfaces).

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static u
address 192.168.20.9
netmask 255.255.255.0
gateway 192.0.2.254
```

## Add MAC Hardware Address to Network Interface

ifconfig

```
ifconfig eth0 hw ether 00:0c:29:33:4e:aa
```

ip

```
ip link set dev eth0 address 00:0c:29:33:4e:aa
```

## Enabling or Disabling Network Interface

ifconfig

```
ifconfig eth0 down
ifconfig eth0 up
```

ip

```
ip link set dev eth0 down
ip link set dev eth0 up
```

## Enable\Disable ARP Protocol

ifconfig

```
ifconfig eth0 arp
ifconfig eth0 -arp
```

ip

```
ip link set dev eth0 arp on
ip link set dev eth0 arp off
```

## Gateway and Routes

route

```
route add default gw <ip>
route add -net 192.168.10.0 netmask 255.255.255.0 dev eth0
route
```

ip

```
ip route add default via <gw addr>
ip route add <ip> via <gw addr> dev <interface>
ip route show
```

## DHCP

Request IP Address (Sends broadcast)

```
dhclient
```

More advanced request, forces IPv4, against the specified server for the specified interface

```
dhclient -4 -s 10.128.128.128 eth0
```

## vConfig

Check if 8021q is loaded:

```
lsmod | grep 8021q
```

If not:

```
modprobe 8021q
```

Add vConfig

```
vconfig add eth0 5
```

Check:

```
ifconfig eth0.5
```

```
ifconfig eth0.5 192.168.1.100 netmask 255.255.255.0 broadcast 192.168.1.255 up
ifconfig eth0.5 x.x.x.x netmask x.x.x.x broadcast x.x.x.x up
```

To delete:

```
ifconfig eth0.5 down
vconfig rem eth0.5
```

OR:

```
ip link add link eth0 name eth0.5 type vlan id 5
ip link
ip -d link show eth0.5
```

You need to activate and add an IP address to vlan link, type:

```
ip addr add 192.168.1.200\24 brd 192.168.1.255 dev eth0.5
ip link set dev eth0.5 up
```

To remove:

```
ip link set dev eth0.5 down
ip link delete eth0.5
```