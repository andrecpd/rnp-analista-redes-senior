# Cases de Redes

## Case 01 — BGP Established, prefixo não chega
Hipóteses:
- prefix-list
- route-map
- policy
- address-family
- next-hop
- anúncio inexistente
- community
- RIB/FIB

## Case 02 — OSPF não forma Full
Verificar:
- área
- timers
- MTU
- autenticação
- network type
- router-id
- conectividade
- ACL

## Case 03 — Usuários de uma VLAN sem acesso
Verificar:
1. porta access
2. VLAN criada
3. VLAN permitida no trunk
4. STP
5. gateway SVI
6. DHCP
7. ARP
8. rota
9. ACL

## Case 04 — Backbone congestionado
Analisar:
- interfaces e capacidade
- percentuais de utilização
- top talkers
- horários
- crescimento
- QoS
- caminhos alternativos
- capacidade futura

A decisão deve considerar risco, custo, capacidade e janela de mudança.
