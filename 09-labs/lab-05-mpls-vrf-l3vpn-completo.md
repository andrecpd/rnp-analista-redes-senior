# LAB 05 — MPLS + VRF + L3VPN

## Topologia

~~~text
CE1 --- PE1 --- P --- PE2 --- CE2
             MPLS
~~~

## Loopbacks

| Equipamento | Loopback |
|---|---|
| PE1 | 10.255.0.1/32 |
| P | 10.255.0.2/32 |
| PE2 | 10.255.0.3/32 |

## Customer A

- CE1 LAN: 192.168.10.0/24
- CE2 LAN: 192.168.20.0/24
- VRF: CUSTOMER-A
- RD: 65000:100
- RT import/export: 65000:100

## Conceitos

MPLS, label, LSR, LER, LDP, VRF, RD, RT, MP-BGP, VPNv4, control plane e data plane.

## PE1 — exemplo

~~~cisco
ip vrf CUSTOMER-A
 rd 65000:100
 route-target export 65000:100
 route-target import 65000:100

interface g0/1
 ip vrf forwarding CUSTOMER-A
 ip address 192.168.10.1 255.255.255.0
 no shutdown
~~~

Em IOS XE moderno, adapte para vrf definition se necessário.

## Core

~~~cisco
mpls ip

interface g0/0
 mpls ip

mpls ldp router-id Loopback0 force
~~~

## Validação

~~~text
show mpls ldp neighbor
show mpls forwarding-table
show ip route
show ip route vrf CUSTOMER-A
show ip bgp vpnv4 all summary
show ip bgp vpnv4 vrf CUSTOMER-A
~~~

## Falha 1 — LDP

Interrompa LDP e determine o impacto no forwarding.

## Falha 2 — RT

Remova temporariamente RT import no PE2.

Pergunta: a rota ainda existe no MP-BGP? Aparece na VRF?

## Falha 3 — VRF

Compare:

~~~text
show ip route
show ip route vrf CUSTOMER-A
~~~

Explique como tabelas diferentes podem conter redes sobrepostas.

## Falha 4 — PE-CE

Derrube o enlace CE-PE e diferencie:
- CE-PE
- MP-BGP
- LDP
- forwarding

## Desafio

Explique de ponta a ponta:

CE → PE → label → P → PE → VRF → CE

E por que o roteador P não precisa conhecer a rota IPv4 do cliente.
