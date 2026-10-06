# LAB 02 — BGP Completo + Policy

## Topologia

~~~text
AS65001 ---- AS65002 ---- AS65003
   R1          R2           R3
~~~

## Endereçamento

| Link | Rede |
|---|---|
| R1-R2 | 10.10.12.0/30 |
| R2-R3 | 10.10.23.0/30 |
| R1 Loopback | 192.0.2.1/32 |
| R2 Loopback | 192.0.2.2/32 |
| R3 Loopback | 192.0.2.3/32 |

## R1 — AS65001

~~~cisco
router bgp 65001
 bgp router-id 192.0.2.1
 neighbor 10.10.12.2 remote-as 65002
 network 172.16.1.0 mask 255.255.255.0

interface Loopback10
 ip address 172.16.1.1 255.255.255.0
~~~

## R2 — AS65002

~~~cisco
router bgp 65002
 bgp router-id 192.0.2.2
 neighbor 10.10.12.1 remote-as 65001
 neighbor 10.10.23.2 remote-as 65003
~~~

## R3 — AS65003

~~~cisco
router bgp 65003
 bgp router-id 192.0.2.3
 neighbor 10.10.23.1 remote-as 65002
 network 172.16.3.0 mask 255.255.255.0
~~~

## Validação

~~~text
show ip bgp summary
show ip bgp
show ip bgp <prefix>
show ip route bgp
show ip bgp neighbors
~~~

## Prefix-list

~~~cisco
ip prefix-list ONLY-ROUTES seq 5 permit 172.16.1.0/24
~~~

Teste filtro inbound e outbound.

## Route-map / Local Preference

~~~cisco
route-map SET-LP permit 10
 set local-preference 200
~~~

Explique por que maior Local Preference é preferido dentro do AS.

## Falha 1 — Sessão Established, prefixo ausente

Filtre o prefixo.

Pergunta: como diferenciar problema de sessão BGP de problema de anúncio/política?

## Falha 2 — Prefixo no BGP, mas não na RIB

Investigue:

~~~text
show ip bgp <prefix>
show ip route <prefix>
show ip route <next-hop>
~~~

Explique next-hop recursivo.

## Falha 3 — Dois caminhos

Faça o mesmo prefixo chegar por dois caminhos e compare:
1. Weight
2. Local Preference
3. rota local
4. AS_PATH
5. Origin
6. MED
7. eBGP/iBGP
8. métrica IGP

## Desafio

Explique:
- eBGP x iBGP
- AS_PATH
- Local Preference
- MED
- next-hop-self
- sessão Established sem rota
- prefix-list
- route-map
