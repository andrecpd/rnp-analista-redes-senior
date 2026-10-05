# Quick Reference — RNP Analista de Redes Sênior

## 1. Cisco — interface/L2

```text
show ip interface brief
show interfaces
show interfaces counters errors
show vlan brief
show interfaces trunk
show mac address-table
show spanning-tree
show etherchannel summary
show cdp neighbors detail
```

**Pergunta que responde:** link, VLAN, trunk, MAC, STP ou EtherChannel?

## 2. Cisco — L3

```text
show ip route
show ip route <prefix>
show ip arp
show ip cef <prefix>
show ip interface brief
```

**Fluxo:** interface → neighbor → rota → CEF/FIB.

## 3. OSPF

```text
show ip ospf
show ip ospf neighbor
show ip ospf interface
show ip ospf interface brief
show ip ospf database
show ip route ospf
```

Se não está Full:
`interface → area → timers → MTU → auth → network type → ACL`

## 4. BGP

```text
show ip bgp summary
show ip bgp
show ip bgp <prefix>
show ip bgp neighbors
show ip bgp neighbors <IP> advertised-routes
show ip bgp neighbors <IP> received-routes
show ip route <prefix>
show ip route <next-hop>
```

> Alguns comandos/outputs variam por plataforma e configuração.

## 5. MPLS / VRF

```text
show vrf
show ip route vrf <VRF>
show mpls interfaces
show mpls ldp neighbor
show mpls ldp bindings
show mpls forwarding-table
show ip bgp vpnv4 all summary
show ip bgp vpnv4 all
```

## 6. IPv6

```text
show ipv6 interface brief
show ipv6 interface
show ipv6 neighbors
show ipv6 route
ping ipv6 <destino>
traceroute ipv6 <destino>
show ipv6 ospf neighbor
show ipv6 ospf database
```

## 7. Linux

```bash
ip -br addr
ip -br link
ip route
ip route get <destino>
ip neigh
ss -lntup
ping -c 4 <destino>
traceroute <destino>
mtr -rw <destino>
dig <nome>
curl -v <url>
tcpdump -ni any host <ip>
journalctl -u <servico>
systemctl status <servico>
```

## 8. Captura

```bash
tcpdump -ni eth0 port 443
tcpdump -ni eth0 host 10.10.10.10
tcpdump -ni eth0 'tcp[tcpflags] & tcp-syn != 0'
```

## 9. Diagnóstico em 60 segundos

```text
1. Interface UP?
2. VLAN/L2 correto?
3. ARP/NDP existe?
4. Gateway responde?
5. Rota existe?
6. Next-hop é alcançável?
7. ACL/firewall?
8. TCP/UDP?
9. DNS?
10. Aplicação?
```

## 10. Pegadinhas

- `ping` ≠ aplicação.
- `traceroute` ≠ prova absoluta de falha.
- BGP Established ≠ prefixo instalado.
- OSPF Full ≠ todas as rotas presentes.
- `LISTEN` ≠ aplicação saudável.
- Alta CPU ≠ causa raiz automaticamente.

**Golden rule:** use o comando que responde à sua hipótese.
