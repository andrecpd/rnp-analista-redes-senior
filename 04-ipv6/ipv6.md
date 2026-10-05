# IPv6

## Conceitos
IPv6 possui 128 bits.

Tipos:
- Global Unicast: roteável globalmente.
- Link-Local: FE80::/10.
- Unique Local: FC00::/7.
- Multicast: FF00::/8.
- Loopback: ::1.
- Unspecified: ::.

## ICMPv6 / NDP
NDP substitui funções do ARP e usa ICMPv6.

Mensagens:
- Router Solicitation
- Router Advertisement
- Neighbor Solicitation
- Neighbor Advertisement
- Redirect

## SLAAC
O host pode formar endereço usando informações recebidas em Router Advertisement.

## Troubleshooting
```text
show ipv6 interface
show ipv6 neighbors
show ipv6 route
ping ipv6 <endereco>
traceroute ipv6 <endereco>
```

Atenção: IPv6 depende fortemente de ICMPv6; bloquear ICMPv6 indiscriminadamente pode quebrar funcionalidades.
