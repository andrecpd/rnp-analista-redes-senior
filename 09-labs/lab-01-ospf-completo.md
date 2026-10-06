# LAB 01 — OSPF Completo + Troubleshooting

## Objetivo

Praticar OSPF em topologia redundante e diagnosticar adjacência, Router-ID, LSDB, custo, DR/BDR, MTU, passive-interface e reconvergência.

## Topologia

~~~text
              R2
             /  \
            /    \
          R1------R3
~~~

Todos os enlaces estão na Area 0.

## Endereçamento

| Link | Rede |
|---|---|
| R1-R2 | 10.0.12.0/30 |
| R2-R3 | 10.0.23.0/30 |
| R1-R3 | 10.0.13.0/30 |
| R1 Loopback | 1.1.1.1/32 |
| R2 Loopback | 2.2.2.2/32 |
| R3 Loopback | 3.3.3.3/32 |

## R1

~~~cisco
hostname R1
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
interface GigabitEthernet0/0
 description R1-R2
 ip address 10.0.12.1 255.255.255.252
 ip ospf 1 area 0
 no shutdown
interface GigabitEthernet0/1
 description R1-R3
 ip address 10.0.13.1 255.255.255.252
 ip ospf 1 area 0
 no shutdown
router ospf 1
 router-id 1.1.1.1
 passive-interface Loopback0
 auto-cost reference-bandwidth 10000
~~~

## R2

~~~cisco
hostname R2
interface Loopback0
 ip address 2.2.2.2 255.255.255.255
interface GigabitEthernet0/0
 ip address 10.0.12.2 255.255.255.252
 ip ospf 1 area 0
 no shutdown
interface GigabitEthernet0/1
 ip address 10.0.23.1 255.255.255.252
 ip ospf 1 area 0
 no shutdown
router ospf 1
 router-id 2.2.2.2
 passive-interface Loopback0
 auto-cost reference-bandwidth 10000
~~~

## R3

~~~cisco
hostname R3
interface Loopback0
 ip address 3.3.3.3 255.255.255.255
interface GigabitEthernet0/0
 ip address 10.0.23.2 255.255.255.252
 ip ospf 1 area 0
 no shutdown
interface GigabitEthernet0/1
 ip address 10.0.13.2 255.255.255.252
 ip ospf 1 area 0
 no shutdown
router ospf 1
 router-id 3.3.3.3
 passive-interface Loopback0
 auto-cost reference-bandwidth 10000
~~~

## Validação

~~~text
show ip ospf neighbor
show ip ospf database
show ip route ospf
show ip ospf interface brief
show ip ospf interface
ping 3.3.3.3 source 1.1.1.1
traceroute 3.3.3.3 source 1.1.1.1
~~~

## Exercício — Custo

No R1:

~~~cisco
interface g0/1
 ip ospf cost 100
~~~

Observe a mudança para 3.3.3.3. Depois retorne o custo para 1.

Explique por que o caminho mudou.

## Exercício — Falha de enlace

~~~cisco
interface g0/1
 shutdown
~~~

Valide neighbor, RIB e traceroute.

## Exercício — MTU

R1:

~~~cisco
interface g0/0
 ip mtu 1400
~~~

R2:

~~~cisco
interface g0/0
 ip mtu 1500
~~~

Perguntas:
1. Qual estado aparece?
2. O Hello continua chegando?
3. Qual comando confirma a diferença?
4. Qual correção aplicar?

## Desafio

Explique sem consultar:
- Router-ID
- LSDB
- Neighbor x Adjacency
- DR/BDR
- LSA
- custo
- reconvergência

## Entrega

~~~text
Sintoma:
Comando:
Evidência:
Hipótese:
Teste:
Causa:
Correção:
Resultado:
~~~