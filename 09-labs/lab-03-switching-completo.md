# LAB 03 — Switching: VLAN + Trunk + STP + EtherChannel

## Topologia

~~~text
PC1 --- SW1 ===== SW2 --- PC2
          ||       ||
          ===== redundancy =====
~~~

## VLANs

| VLAN | Nome | Rede |
|---|---|---|
| 10 | USERS | 10.10.10.0/24 |
| 20 | SERVERS | 10.10.20.0/24 |
| 99 | MGMT | 10.10.99.0/24 |

## Configuração

~~~cisco
vlan 10
 name USERS
vlan 20
 name SERVERS
vlan 99
 name MGMT

interface g0/1
 switchport mode access
 switchport access vlan 10

interface g0/2
 switchport mode access
 switchport access vlan 20

interface g0/24
 description TRUNK-SW2
 switchport mode trunk
 switchport trunk allowed vlan 10,20,99
~~~

## Validação

~~~text
show vlan brief
show interfaces trunk
show mac address-table
show interfaces status
~~~

## RSTP

~~~cisco
spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,99 root primary
~~~

No segundo switch:

~~~cisco
spanning-tree vlan 10,20,99 root secondary
~~~

Valide:

~~~text
show spanning-tree
show spanning-tree vlan 10
~~~

## EtherChannel — LACP

~~~cisco
interface range g0/23-24
 channel-group 1 mode active

interface Port-channel1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,99
~~~

## Falha 1 — VLAN ausente

Remova VLAN 20 do trunk e prove por que apenas VLAN 20 perde conectividade.

## Falha 2 — Native VLAN

Configure native VLAN diferente nos dois lados e observe os logs.

## Falha 3 — EtherChannel

Deixe um membro com configuração diferente.

~~~text
show etherchannel summary
show interfaces port-channel 1
show lacp neighbor
~~~

## Falha 4 — Loop L2

Crie caminho redundante e observe STP.

Explique:
- root bridge
- root port
- designated port
- estados
- convergência
