# IPv6 prefix delegation in layer 3 mesh networks

## Problem 1

How do we distribute the IPv6 public prefix from a gateway to nodes in the
network? The problem is that we can't just use DHCPv6 or NDP because we
don't have one big layer-2-network.

## Problem 2

How do we guarantee that packets with a specific source address get routed
via the gateway the address originated from? The problem is that a gateway or
upstream ISP would possibly drop outbound packages that do not have a src ip
from the gateway prefix. This makes sense to mitigate e.g. DDoS attacks based
on source address spoofing.

## Problem

- public prefix owned by an internet gateway
- nodes in the network want to route data via the gateway
- nodes want/need to use a sub-prefix of the gateways prefix
- src address must be subprefix /128 of gateways public prefix
- avoid NAT if possible

## Ideas for solutions

- idependent mechanism that configures prefixes in the network
- DHCPv6 over a layer 2 tunnel from the node to the gateway
- unicast request over DHCPv6 relay on the node to the gateway
- normal DHCPv6 prefix delegation from node to node
- NPTv6 Network Prefix Translation

### Assumption about the network

I assume that we can establish basic connectivity between nodes in the mesh
network by using ULA prefixes and a routing daemon that uses these addresses for
establishing a mesh network.

ULA: https://tools.ietf.org/html/rfc4193

### DHCPv6 over layer 2 tunnel

A layer 2 tunnel has an ethernet header in the inner header. Works on layer 2 of
the OSI model.

### Unicast DHCPv6 request over DHCPv6 relay

## Terminology

- node
- gateway
- link
- site?
- network
- prefix
  - public
  - private
  - ULA

## Protocols

### Tunneling

- GRE6TAP
- L2TPv3
- IP-GUE
- fastd?

### Prefix delegation IPv6

- DHCPv6
- HNCP https://tools.ietf.org/html/rfc7788

### Routing

Source specific routing is a requirement for at least one of the proposed
solutions.

- babel
- OLSRv2
- (bmx)

## Random ideas

State about the prefix delegation is only of interest for the gateway that
shares the prefix and the node that uses the prefix.

A gateway should announce that a prefix is available for delegation.

Maybe a gateway should announce a prefix and the sub-prefixes it would like/can
share.

Announcement should contain the prefix and a list of (available sub-prefixes).

## Related mailing list conversations

- https://ml.ninux.org/pipermail/battlemesh/2016-February/004352.html
- https://ml.ninux.org/pipermail/battlemesh/2016-April/004700.html


## Final remarks

With a layer 2 routing protocol such as BATMAN prefix delegation is kind of
trivial.
