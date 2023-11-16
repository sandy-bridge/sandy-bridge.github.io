---
author: sandy-bridge
categories:
- Uncategorised
date: "2018-12-19T23:22:00Z"
guid: https://www.sandybridge.me/2018/12/telia-formerly-elion-tv-on-3rd-party.html
id: 131
tags:
- Tech
title: 'Telia (formerly Elion) TV on a 3rd party router: Part 1'
url: /2018/12/19/telia-formerly-elion-tv-on-a-3rd-party-router-part-1/
---

The largest internet service provider in Estonia is Telia Eesti, formerly operating as Elion. Along with broadband internet, they provide a TV service, using IPTV technology. Since this is a rather new technology, it has yet to be standardised, and every ISP operates it with their own parameters. As a result of that, customers are often required to use a router provided by the ISP, in this case Telia, to gain access to IPTV services, since they are distributed via the same broadband connection. However, with a bit of work, one can use IPTV with a router obtained through another party on Telia.

## **A bit of background**

  
IPTV, or Internet Protocol television, is a term describing the delivery of television content via the Internet Protocol. This is different from internet television, which is the delivery of content over the Internet. Even Telia offers such a service – minuTV. IPTV differs from that since it is only distributed over a restricted network (Telia’s IPTV network), and requires a special device to view (a set-top box from Telia, such as an [Arris VIP5305](https://pood.telia.ee/productInfo/426/digiboks-arris-vip5305/612138-003-00).) Telia’s closed IPTV network is accessible to everyone who has this service activated under VLAN ID 4. The [Inteno DG200A](https://pood.telia.ee/productInfo/431/ruuter-inteno-dg200a/DG200A), [DG301](https://pood.telia.ee/productInfo/431/ruuter-inteno-dg301/DG301A) and [DG400P](https://pood.telia.ee/productInfo/431/ruuter-inteno-dg400p/DG400P-TELIA) routers offered by Telia are pre-configured to connect to that VLAN, and the STBs on the local home network get their content by proxy from the router.

Live TV channels are distributed as multicast groups, each with their own IP address, such as 239.3.1.1. To join one of these groups, an STB sends a request via the IGMP (Internet Group Management Protocol) to the entire network. The router receives the response, and forwards the request to the IPTV network, where the channel becomes available to the router. A live video stream will then be continuously sent to the whole local network, and it’s up to switches on the network to filter out the stream (IGMP snooping). Telia’s routers do this automatically, but if the STB is behind another switch, it might not do that. The stream will get to the STB, which will decrypt, decode the stream and play it on a connected screen. Recorded programmes, rental movies, and other non-live content is available by a standard video stream from the IPTV network. For this, the STB simply requests content directly from the server, and the router is set up to forward that request to the IPTV network, rather than the Internet.

## **Prerequisites**

  
To use Telia TV with a 3rd party one router, one must first have the Telia TV service activated on their home broadband connection. This is tutorial only applicable to DSL or Fibre connections, it is not required nor possible on F4G. One must also have a 3rd party router, however special requirements need to be considered when buying one. It must support custom firmware, or be configurable to connect to VLAN 4 on the WAN side, in addition to the untagged internet VLAN. I personally used [OpenWRT](https://openwrt.org/), however people have had success with earlier versions of [DD-WRT](https://dd-wrt.com/) as well. Information on hardware compatibility and flashing instructions can be found on their respective websites.

**Sandy Bridge, nor the author of this blog is not responsible for any damages caused by modifying critical system components of any router, gateway, CPE, or any other piece of network equipment. Flash firmware at your own risk!**  
  
This is the end of part one. Part two will be on 2 ways to set up IPTV, with detailed instructions for both, on OpenWRT.