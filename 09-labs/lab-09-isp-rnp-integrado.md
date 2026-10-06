# LAB 09 — ISP / RNP Integrado

## Objetivo

Unir os conhecimentos em uma arquitetura de provedor.

## Topologia

~~~text
                 INTERNET / RNP
                       |
                    BGP-EDGE
                       |
              +--------+--------+
              |                 |
             PE1---------------PE2
              |       MPLS      |
              +-------P---------+
               |               |
              SW1-------------SW2
               |               |
             USERS           SERVERS
~~~

## Componentes

BGP, OSPF, MPLS, VRF, VLAN, STP, IPv4, IPv6, Linux e monitoramento.

## Objetivos

1. OSPF fornece reachability das loopbacks.
2. MPLS transporta o core.
3. VRF separa clientes.
4. MP-BGP transporta VPNv4.
5. BGP fornece conectividade externa.
6. VLAN separa usuários e servidores.
7. IPv6 opera em paralelo.
8. Linux fornece serviços de teste.
9. Monitoramento detecta falhas.

## Validação L2

~~~text
show vlan brief
show interfaces trunk
show spanning-tree
show etherchannel summary
~~~

## Validação IGP

~~~text
show ip ospf neighbor
show ip route
~~~

## Validação BGP

~~~text
show ip bgp summary
show ip bgp
~~~

## Validação MPLS

~~~text
show mpls ldp neighbor
show mpls forwarding-table
~~~

## Validação VRF

~~~text
show ip route vrf <VRF>
~~~

## Validação IPv6

~~~text
show ipv6 interface brief
show ipv6 route
~~~

## Validação Linux

~~~bash
ip addr
ip route
ss -lntup
tcpdump
~~~

## Falhas

Aplique:
1. OSPF neighbor down
2. BGP Established sem prefixo
3. VLAN removida do trunk
4. RT import removido
5. LDP interrompido
6. rota IPv6 removida
7. porta TCP fechada

## Entrega

~~~text
Arquitetura:
Objetivo:
Topologia:
Endereçamento:
Protocolos:
Baseline:
Falhas:
Evidências:
Causa raiz:
Correções:
Validação:
Lições aprendidas:
~~~
