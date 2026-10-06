# LAB 04 — IPv4 + IPv6 + NDP + ICMPv6

## Topologia

~~~text
R1 -------- R2 -------- Linux1
~~~

## Prefixos

| Link | Prefixo |
|---|---|
| R1-R2 | 2001:db8:12::/64 |
| R2-Linux | 2001:db8:20::/64 |
| R1 Loopback | 2001:db8:1::1/128 |
| R2 Loopback | 2001:db8:2::1/128 |

## Roteador

~~~cisco
ipv6 unicast-routing

interface g0/0
 ipv6 address 2001:db8:12::1/64
 no shutdown

interface Loopback0
 ipv6 address 2001:db8:1::1/128

ipv6 route 2001:db8:20::/64 2001:db8:12::2
~~~

## Linux

~~~bash
ip -6 addr
ip -6 route
ip -6 neigh
ping -6 2001:db8:2::1
~~~

## NDP

~~~bash
tcpdump -ni any icmp6
~~~

Identifique NS, NA, RS e RA.

## SLAAC

Fluxo:

RS → RA → Prefixo → Endereço → Default Route

## Falhas

1. Remover default route IPv6.
2. Investigar NDP.
3. Interferir em RA.
4. Bloquear ICMPv6 e observar o impacto.

## Perguntas

1. IPv6 usa ARP?
2. Para que serve NDP?
3. O que é link-local?
4. O que é SLAAC?
5. Função do RA?
6. Por que ICMPv6 é importante?
