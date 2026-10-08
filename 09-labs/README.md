# LABS — Analista de Redes Sênior | RNP

Esta pasta contém laboratórios práticos para preparação técnica de Analista de Redes Sênior.

## Metodologia

Todo laboratório segue:

Baseline → Configuração → Validação → Falha proposital → Evidência → Hipótese → Correção → Validação final

O objetivo não é apenas fazer o ping funcionar. É saber explicar por que funciona, por que parou e como provar a causa.

## Requisitos

- EVE-NG ou GNS3
- Cisco IOSv/IOSvL2 quando disponível
- Linux Ubuntu/Debian ou FRRouting
- Wireshark
- SSH

## Mapa

| Lab | Tema | Nível |
|---|---|---|
| 01 | OSPF + Troubleshooting | Intermediário |
| 02 | BGP + Policy | Avançado |
| BGP-01 | [eBGP básico entre dois ASNs](bgp/01-ebgp-basico/README.md) | Avançado |
| 03 | Switching + VLAN + STP + EtherChannel | Intermediário |
| 04 | IPv4/IPv6 + ICMPv6/NDP | Intermediário |
| 05 | MPLS + VRF + L3VPN | Avançado |
| 06 | Linux Networking + TCP/DNS | Intermediário |
| 07 | Troubleshooting integrado | Avançado |
| 08 | Monitoramento + SNMP/Syslog | Intermediário |
| 09 | ISP/RNP integrado | Avançado |
| 10 | Simulado prático de 2 horas | Prova |

## Trilha BGP detalhada

A trilha progressiva de BGP, com configurações FRR, troubleshooting e gabarito do primeiro laboratório, está em [09-labs/bgp/README.md](bgp/README.md).

## Ordem recomendada

1. Switching
2. OSPF
3. IPv4/IPv6
4. BGP
5. MPLS/VRF
6. Linux
7. Monitoramento
8. Troubleshooting
9. Lab integrado
10. Simulado

## Checklist

- [ ] Montar topologia sem consultar
- [ ] Configurar protocolos
- [ ] Validar estado operacional
- [ ] Coletar evidências
- [ ] Identificar falha
- [ ] Explicar causa raiz
- [ ] Corrigir
- [ ] Validar
- [ ] Documentar

## Comandos essenciais

### Cisco IOS

~~~text
show ip interface brief
show interfaces
show ip route
show arp
show cdp neighbors
show lldp neighbors
show logging
show running-config
~~~

### Linux

~~~bash
ip -br addr
ip route
ip route get <IP>
ip neigh
ss -lntup
ping
traceroute
mtr
dig
curl -v
tcpdump
~~~

## Evidência obrigatória

~~~text
Sintoma:
Impacto:
Evidência:
Hipótese:
Teste:
Causa raiz:
Correção:
Validação:
Lição aprendida:
~~~
