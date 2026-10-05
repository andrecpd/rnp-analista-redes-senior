# Cases de Redes — Nível Sênior

## Como responder

Use sempre:

**Contexto → Sintoma → Escopo → Evidência → Hipóteses → Testes → Causa raiz → Correção → Validação → Prevenção**

---

## CASE 01 — BGP Established, prefixo não chega

### Sintoma
Sessão BGP está Established, mas `10.20.30.0/24` não aparece na tabela.

### Investigue
```text
show ip bgp summary
show ip bgp 10.20.30.0/24
show ip route 10.20.30.0/24
show ip bgp neighbors <IP>
```

### Hipóteses
- não anunciado;
- AFI/SAFI;
- prefix-list;
- route-map;
- community;
- next-hop;
- perdeu best-path;
- RIB/FIB.

### Resposta
Não reiniciar a sessão antes de localizar onde o prefixo desapareceu.

---

## CASE 02 — OSPF não forma Full

### Evidências
```text
show ip ospf neighbor
show ip ospf interface
show ip ospf database
```

### Hipóteses
- área;
- MTU;
- timers;
- autenticação;
- network type;
- Router ID;
- conectividade;
- ACL.

### Pegadinha
**ExStart → MTU é uma das primeiras verificações.**

---

## CASE 03 — Usuários de uma VLAN sem acesso

### Fluxo
`Access → VLAN → Trunk → STP → SVI → DHCP/ARP → Route → ACL → Aplicação`

### Evidências
```text
show vlan brief
show interfaces trunk
show spanning-tree
show mac address-table
show ip interface brief
show ip arp
```

---

## CASE 04 — Backbone congestionado

### Perguntas
- Qual interface?
- Entrada ou saída?
- Pico ou contínuo?
- Existem drops?
- Quem são os top talkers?
- Existe caminho alternativo?
- QoS está descartando?
- Qual crescimento previsto?

### Decisão
Otimizar → balancear → expandir → redesenhar.

A decisão deve considerar **risco + custo + capacidade + janela + crescimento**.

---

## CASE 05 — Alta latência

Compare:
`origem → trânsito → destino`

Use:
- ping;
- traceroute;
- mtr;
- métricas;
- captura.

**Não declare culpa de um hop apenas por latência ICMP alta.**

---

## CASE 06 — Perda intermitente

Investigue:
- CRC;
- drops;
- congestionamento;
- policer;
- QoS;
- MTU;
- CPU;
- flaps;
- assimetria;
- firewall.

Pergunta importante:

> A perda aparece também no destino ou somente em um hop intermediário?

---

## CASE 07 — IPv6 sem comunicação

Fluxo:
`Address → Prefix → NDP → RA/Default Route → Routing → ICMPv6 → ACL → MTU`

Comandos:
```text
show ipv6 interface
show ipv6 neighbors
show ipv6 route
ping ipv6 <destino>
```

---

## CASE 08 — L3VPN MPLS sem comunicação

Fluxo:
`CE-PE → VRF → Route → RT → MP-BGP → Next-hop → LDP/LSP → Labels → Forwarding`

### Pegadinha
- RD distingue.
- RT controla import/export.

---

## CASE 09 — MAC flapping

### Hipóteses
- loop L2;
- EtherChannel inconsistente;
- STP;
- cabos;
- redundância conectada incorretamente.

### Evidências
```text
show mac address-table
show spanning-tree
show etherchannel summary
```

---

## CASE 10 — TCP indisponível

Separe:

`DNS → IP → ARP/NDP → TCP → TLS → HTTP → aplicação`

No Linux:
```bash
ss -lntup
nc -vz <ip> <porta>
curl -v https://<host>
tcpdump -ni any host <ip> and port <porta>
```

---

## CASE 11 — OSPF/BGP caiu após mudança

Pergunte:
1. O que mudou?
2. Em que horário?
3. Qual estado mudou primeiro?
4. O log confirma?
5. O rollback é seguro?
6. Como validar após rollback?

**Correlação temporal é evidência; coincidência não é prova.**

---

## CASE 12 — Incidente crítico

Resposta sênior:
1. confirmar impacto;
2. definir escopo;
3. preservar evidências;
4. estabilizar serviço;
5. evitar mudanças paralelas;
6. comunicar status;
7. corrigir/rollback;
8. validar;
9. documentar;
10. criar prevenção.

## Regra final

> Não diga apenas "eu verificaria". Diga **o que verificaria, qual evidência espero encontrar e qual decisão tomaria dependendo do resultado**.
