# Quick Reference

## Cisco
```text
show ip interface brief
show interfaces
show ip route
show ip arp
show cdp neighbors detail
show vlan brief
show interfaces trunk
show spanning-tree
show etherchannel summary
show ip ospf neighbor
show ip ospf database
show ip bgp summary
show ip bgp
show ip bgp neighbors
show logging
show processes cpu
show memory
```

## Linux
```bash
ip addr
ip route
ip neigh
ss -lntup
ping
traceroute
mtr
dig
curl
tcpdump
journalctl
systemctl status <servico>
```

## Diagnóstico rápido
Interface → VLAN → ARP/NDP → gateway → rota → ACL/firewall → transporte → DNS → aplicação.
