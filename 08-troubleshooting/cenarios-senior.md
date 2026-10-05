# Cenários de Troubleshooting — Nível Sênior

## 1 — BGP Established, sem rota
Verifique sessão, anúncio, address-family, prefix-list, route-map, communities, next-hop e RIB/FIB. Não confunda sessão TCP/BGP ativa com troca efetiva de rotas.

## 2 — OSPF preso em ExStart
Investigue MTU, network type, timers, autenticação, Router ID e conectividade.

## 3 — VLAN sem acesso
Fluxo: porta access → VLAN → trunk → STP → SVI/gateway → ARP → DHCP → rota → ACL/firewall → aplicação.

## 4 — Backbone saturado
Colete utilização, erros/discards, top talkers, horários, capacidade, QoS, caminhos alternativos e crescimento. Decida entre otimização, balanceamento, expansão ou mudança arquitetural.

## 5 — Alta latência
Compare origem, trânsito e destino. Use ping, traceroute/mtr e métricas de interface. Não atribua causa apenas a um hop isolado.

## 6 — Perda intermitente
Investigue físico, congestionamento, policers, QoS, MTU, CPU, assimetria e flaps.

## 7 — IPv6 sem comunicação
Verifique endereços, link-local, NDP, RA/SLAAC, rota IPv6, ICMPv6 e filtros.

## 8 — L3VPN MPLS sem comunicação
Verifique CE-PE, VRF, rotas locais, MP-BGP, RT import/export, LDP/LSP e labels.

## 9 — MAC flapping
Verifique loops, redundância L2, STP, EtherChannel, cabos e topologia.

## 10 — Serviço TCP indisponível
Separe DNS → reachability IP → TCP → TLS → HTTP/aplicação → autenticação.

## Regra de resposta
Contexto → sintoma → evidências → hipóteses → testes → causa raiz → ação → validação → prevenção.