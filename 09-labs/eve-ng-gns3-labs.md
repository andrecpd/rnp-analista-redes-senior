# Labs EVE-NG / GNS3 — Preparação RNP

## Objetivo

Treinar **configuração + observabilidade + falha proposital + diagnóstico**.

Regra do laboratório:

`Baseline → Falha → Evidência → Hipótese → Correção → Validação`

Não vale apenas "fazer funcionar".

---

## LAB 01 — OSPF

### Topologia
`R1 — R2 — R3`

- Area 0
- loopbacks
- interfaces ponto a ponto
- custo OSPF

### Treino
1. formar adjacências;
2. verificar LSDB;
3. alterar cost;
4. derrubar link;
5. observar reconvergência.

### Comandos
```text
show ip ospf neighbor
show ip ospf database
show ip route ospf
show ip ospf interface
```

### Falha proposital
Configure MTU diferente entre R1/R2.

**Pergunta:** em qual estado a adjacência pode ficar e qual evidência confirma a hipótese?

---

## LAB 02 — BGP

### Topologia
`AS65001 — AS65002 — AS65003`

Treine:
- eBGP;
- loopbacks;
- anúncio de prefixos;
- AS_PATH;
- Local Preference;
- MED;
- prefix-list;
- route-map.

### Comandos
```text
show ip bgp summary
show ip bgp
show ip bgp <prefix>
show ip bgp neighbors
show ip route <prefix>
```

### Falha proposital
Filtre um prefixo com prefix-list.

**Objetivo:** descobrir se a sessão está Established mesmo sem o prefixo.

---

## LAB 03 — Switching

### Topologia
`SW1 ===== SW2`

- VLAN 10
- VLAN 20
- trunk
- STP/RSTP
- EtherChannel

### Falhas
- remover VLAN do trunk;
- alterar native VLAN;
- criar caminho L2 redundante;
- provocar inconsistência de EtherChannel.

### Comandos
```text
show vlan brief
show interfaces trunk
show spanning-tree
show etherchannel summary
show mac address-table
```

---

## LAB 04 — IPv6

Treine:
- global unicast;
- link-local;
- NDP;
- RS/RA;
- NS/NA;
- SLAAC;
- rota default;
- ICMPv6.

### Comandos Linux
```bash
ip -6 addr
ip -6 route
ip -6 neigh
ping -6 <destino>
tcpdump -ni eth0 icmp6
```

### Falha
Bloqueie/afete RA e observe a perda de rota default.

---

## LAB 05 — MPLS + VRF + L3VPN

### Topologia
`CE1 — PE1 — P — PE2 — CE2`

Treine:
- VRF;
- LDP;
- loopbacks;
- MP-BGP;
- RD;
- RT;
- labels.

### Comandos
```text
show vrf
show ip route vrf <VRF>
show mpls ldp neighbor
show mpls forwarding-table
show ip bgp vpnv4 all summary
show ip bgp vpnv4 vrf <VRF>
```

### Falha
Remova RT import ou bloqueie LDP.

**Objetivo:** distinguir falha de controle de falha de forwarding.

---

## LAB 06 — Linux

Monte dois hosts Linux e uma aplicação TCP.

Treine:
```bash
ip addr
ip route
ip route get <destino>
ip neigh
ss -lntup
dig
curl -v
tcpdump
mtr
```

### Falhas
- rota default;
- DNS;
- porta TCP;
- firewall;
- MTU.

---

## LAB 07 — Desafio final

Crie uma topologia com:
- VLAN;
- OSPF;
- BGP;
- IPv6;
- VRF/MPLS;
- Linux.

Introduza **5 falhas sem informar quais**.

Para cada uma registre:
- sintoma;
- impacto;
- evidência;
- hipótese;
- teste;
- causa raiz;
- correção;
- validação.

## Critério de aprovação

Um lab está concluído somente quando você consegue **explicar por que a falha aconteceu**, não apenas quando o ping volta.
