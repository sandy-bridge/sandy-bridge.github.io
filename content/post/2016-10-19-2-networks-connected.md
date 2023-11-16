---
author: sandy-bridge
categories:
- Uncategorised
date: "2016-10-19T18:17:00Z"
guid: https://www.sandybridge.me/2016/10/2-networks-connected.html
id: 133
tags:
- Tech
title: 2 networks connected
url: /2016/10/19/2-networks-connected/
---

About time I start posting things, and I’m gonna start off big. This right post is of one my greatest projects of all time, and it took me nearly half a year to complete it.

**No, it’s not about running a network cable**  
  
I have two places that I would call home, and generally I spend an equal amount of time in both of them. I’ve liked like almost all of my life, and I’ve had separate computers at the places for about six years. Obviously I’ve needed to have my files accessible from both locations. In the past I’ve used an external hard drive and cloud storage, but they haven’t really worked out.

Since February 2015 I’ve been able to host a dedicated server machine. Before that, the majority of my files was on internal hard drives on both computers, and not really organised. With the server, I could transfer over most of my data and start structuring it. But first, I had to make my local network accessible from the Internet – because one does not simply run SMB over WAN.

[SoftEther](http://www.softether.org/) was my VPN server software of choice. It was very easy to set up, and it seemed to perform well at the time. I set it up for remote access mode, so I could just install a client on any computer, and it could connect to my LAN to access the SMB share with my files. However, after I upgraded the network at my other home – the one that wasn’t hosting the server at the time – I started to notice a drop in performance.

**A faster VPN**  
  
I started making plans for improving VPN performance in early 2016. One of the first steps was to build another server for the second home (hereinafter site). I chose an Intel NUC barebone for my hardware, as I needed something small to hide behind the living room TV, but powerful enough to run Windows Server, since that was my operating system of choice at the time. I finally got it on April 6th. Setting up a site-to-site VPN however, wasn’t an easy task.

Generally there are two types of VPNs – bridged and routed. A bridged VPN uses a virtual network adaptor to connect a device direct to another network, meaning the device would appear to be inside that network. This means it gets full access to all resources, whether it runs over IP or not, and also makes it very easy to set up. Routed VPNs on the other hand, allow IP packets to be routed between networks, without forwarding all frames between them. This allows for more scalability, since the networks won’t be sharing an address pool, and for tighter regulation of traffic flow between the networks.

The selection wasn’t easy. I needed SMB traffic to flow, which uses WINS for name resolution, which does not run over IP. On the other hand, I needed to ensure that devices in these networks get the IP from the address pool they’re supposed to get it from, because traffic to WAN needed to go through the default gateway on the respective network. And that was why I opted for the routed VPN in the end.

**The current setup**  
  
Both networks have fibre-optic internet connections with NAT routers on my end. They are basically just firewalls. DHCP and DNS is handled by my servers. Site 1, the site that had a server first, uses the private IP range 192.168.0.0/24 for its network, and site 2 has 192.168.1.0/24. This means that each network can have up to 254 devices connected.

The default gateway, the router that connects to the internet, uses the first address from the pool, aka 192.168.x.1. The next one in the network is for the site server, and the 3rd is for the VPN gateway, that routes traffic between the two networks. Essentially, it’s a Layer 3 switch. Both default gateways have an entry in their routing table to forward traffic to the other network via the VPN gateway.

**Problems during setup**  
  
Site 2 had a very old router for the Internet connection. It had been there since 2011 and couldn’t keep up with the large number of devices. For some reason, if I set the IP of the server to 192.168.1.2, IPTV would cease functioning. The set-top box would no longer start up. Nevertheless, I was able to set up another router, but that’s something for another post.