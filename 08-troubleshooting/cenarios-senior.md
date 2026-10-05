# Cenários de Troubleshooting — Nível Sênior

## 1. BGP Established, mas prefixo não chega

**Evidências:**
```text
show ip bgp summary
show ip bgp <prefix>
show ip route <prefix>
show ip bgp neighbors <IP>
```

**Hipóteses:**
- prefixo não anunciado;
- address-family inativa;
- prefix-list;
- route-map/policy;
- community;
- next-hop inacessível;
- rota recebida mas perdeu best-path;
- não instalada na RIB/FIB.

**Resposta sênior:** não reiniciar a sessão antes de descobrir onde o prefixo desapareceu.

## 2. OSPF preso em ExStart

Verifique:
1. MTU;
2. network type;
3. timers;
4. autenticação;
5. Router ID;
6. conectividade;
7. ACL/firewall.

**Clássico:** MTU incompatível.

## 3. VLAN sem acesso

Fluxo:
`access port → VLAN → trunk → STP → SVI → DHCP/ARP → routing → ACL → serviço`

Comandos:
```text
show vlan brief
show interfaces trunk
show spanning-tree
show ip interface brief
show ip arp
```

## 4. Backbone saturado

Colete:
- utilização por interface;
- input/output drops;
- erros;
- top talkers;
- horário;
- direção;
- QoS;
- caminhos alternativos;
- crescimento.

Decisões:
- otimização;
- balanceamento;
- QoS;
- expansão;
- mudança arquitetural.

## 5. Alta latência

Compare:
`origem → trânsito → destino`

Não declare "o hop 3 é o problema" somente porque o ICMP respondeu lento. Veja os hops seguintes e o destino.

## 6. Perda intermitente

Investigue:
- CRC/erros;
- congestionamento;
- drops;
- policer;
- QoS;
- MTU;
- CPU;
- flaps;
- assimetria;
- firewall stateful.

## 7. IPv6 sem comunicação

Verifique:
1. endereço global/link-local;
2. prefixo;
3. NDP;
4. RA/SLAAC;
5. rota default;
6. ICMPv6;
7. ACL/firewall;
8. MTU/PMTUD.

## 8. L3VPN MPLS sem comunicação

Verifique:
`CE-PE → VRF → rota → MP-BGP → RT → next-hop → LDP/LSP → labels → forwarding`

Pegadinha:
**RD ≠ RT.**

## 9. MAC flapping

Hipóteses:
- loop L2;
- EtherChannel inconsistente;
- cabos/topologia incorreta;
- STP;
- equipamento conectado em dois pontos.

Verifique:
```text
show mac address-table dynamic
show spanning-tree
show etherchannel summary
```

## 10. TCP indisponível

Separe:
`DNS → IP → TCP SYN/SYN-ACK → TLS → HTTP → aplicação`

Se houver dúvida, capture:
```text
tcpdump -ni any host <IP> and port <PORTA>
```

## 11. Método de resposta

Para qualquer cenário:

**Contexto → Sintoma → Escopo → Evidências → Hipóteses → Testes → Causa raiz → Correção → Validação → Prevenção.**

## 12. Pegadinha sênior

A resposta mais forte normalmente não é "eu mudaria X".

É:

> "Primeiro eu confirmaria X com Y; se o resultado for Z, sigo para a hipótese seguinte."
