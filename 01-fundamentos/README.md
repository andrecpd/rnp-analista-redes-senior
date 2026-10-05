# 01 — Fundamentos de Redes

## Modelo OSI
1. Física — bits, fibra, cobre, rádio.
2. Enlace — Ethernet, MAC, VLAN, STP.
3. Rede — IP, ICMP, routing.
4. Transporte — TCP/UDP.
5. Sessão.
6. Apresentação.
7. Aplicação — DNS, HTTP, SSH, SNMP.

## TCP/IP
- Acesso à rede
- Internet
- Transporte
- Aplicação

## Ethernet
- MAC identifica a interface no domínio L2.
- Switch aprende MAC pela porta de origem.
- Broadcast é encaminhado dentro do domínio de broadcast.
- Roteador separa domínios de broadcast.

## ARP
IPv4 usa ARP para descobrir MAC associado a um IPv4 na rede local.
Comandos úteis:
```text
arp -a
ip neigh
```

## ICMP
Usado para diagnóstico e mensagens de controle.
```text
ping
traceroute
tracert
```

## TCP x UDP
TCP: orientado à conexão, confiável, controle de fluxo/congestionamento.
UDP: sem conexão, menor overhead, sem garantia de entrega.

## Portas importantes
22 SSH | 23 Telnet | 53 DNS | 67/68 DHCP | 80 HTTP | 123 NTP | 161/162 SNMP | 443 HTTPS | 514 Syslog
