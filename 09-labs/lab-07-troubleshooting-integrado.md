# LAB 07 — Troubleshooting Integrado

## Objetivo

Simular uma ocorrência real de NOC/Engenharia.

## Topologia

~~~text
                    ISP
                     |
                    R1
                  /    \
                R2------R3
                |        |
              SW1======SW2
                |        |
              Linux    Server
~~~

Inclua OSPF, BGP, VLAN 10/20, trunk, RSTP, IPv6 e Linux.

## Regra

Não olhar a configuração antes de coletar evidências.

## Cenário A — VLAN

Sintoma: VLAN 10 não acessa o servidor.

Colete:

~~~text
show vlan brief
show interfaces trunk
show mac address-table
show spanning-tree
~~~

## Cenário B — BGP

Colete:

~~~text
show ip bgp summary
show ip bgp
show ip route
show ip route <destino>
~~~

## Cenário C — OSPF

Colete:

~~~text
show ip ospf neighbor
show ip ospf interface
show ip ospf database
show ip route ospf
~~~

## Cenário D — Linux

~~~bash
ip -br addr
ip route
ip neigh
ss -lntup
dig
curl -v
tcpdump
~~~

## Cenário E — IPv6

~~~bash
ip -6 addr
ip -6 route
ip -6 neigh
ping -6 <destino>
tcpdump -ni any icmp6
~~~

## Modelo

~~~text
Sintoma:
Serviço:
Escopo:
Evidência 1:
Evidência 2:
Hipótese:
Teste:
Resultado:
Causa raiz:
Correção:
Validação:
Impacto:
Prevenção:
~~~

## Desafio

Introduza cinco falhas:
1. VLAN não permitida no trunk
2. custo OSPF alterado
3. prefix-list bloqueando prefixo
4. rota IPv6 ausente
5. serviço TCP parado

Resolva na ordem de maior impacto.
