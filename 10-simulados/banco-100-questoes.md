# Banco de 100 Questões — RNP Analista de Redes Sênior

Use este banco para revisão rápida. Tente responder sem consultar material.

## IPv4 e fundamentos
1. Quantos hosts utilizáveis existem em /27? A)14 B)30 C)32 D)62
2. Rede de 192.168.10.70/26? A).0 B).64 C).70 D).128
3. Máscara de /28? A)255.255.255.0 B)255.255.255.128 C)255.255.255.240 D)255.255.255.252
4. IPv4 para MAC? A)DNS B)ARP C)DHCP D)NDP
5. Diagnóstico de conectividade? A)ICMP B)FTP C)SMTP D)SNMP
6. Camada OSI do IP? A)2 B)3 C)4 D)7
7. Endereço privado? A)8.8.8.8 B)172.20.10.1 C)200.1.1.1 D)1.1.1.1
8. Broadcast de 10.10.10.64/27? A).95 B).96 C).127 D).255
9. Longest prefix match seleciona? A)VLAN B)prefixo mais específico C)DNS D)MAC
10. Benefício do VLSM? A)mais broadcast B)uso eficiente de endereços C)elimina roteamento D)elimina ARP

## Switching, VLAN e STP
11. VLAN normalmente representa: A)domínio de broadcast B)rota BGP C)túnel MPLS D)protocolo IP
12. Porta access normalmente transporta: A)uma VLAN B)todas as VLANs C)apenas IPv6 D)apenas BGP
13. Trunk 802.1Q permite: A)várias VLANs B)uma VLAN C)apenas voz D)apenas gestão
14. STP existe para: A)evitar loops L2 B)NAT C)BGP D)DNS
15. Root Bridge é escolhido pelo: A)menor Bridge ID B)maior MAC C)maior IP D)maior prioridade
16. Root Port é: A)melhor caminho até Root B)sempre bloqueada C)porta de usuário D)porta WAN
17. RSTP é: A)evolução do STP com convergência mais rápida B)roteamento C)autenticação D)BGP
18. Comando para VLANs: A)show vlan brief B)show ip bgp C)show ip ospf D)show mpls
19. Comando para trunks: A)show interfaces trunk B)show arp C)show ip route D)show logging
20. VLAN 20 perdeu acesso após mudança de trunk. Primeiro: A)BGP B)VLAN permitida no trunk C)DNS D)MPLS

## OSPF
21. OSPF é: A)IGP link-state B)EGP path-vector C)L2 D)aplicação
22. OSPF utiliza: A)Dijkstra/SPF B)STP C)BGP D)ARP
23. Estado de vizinhança totalmente formada: A)Init B)Exchange C)Full D)Down
24. Em LAN broadcast OSPF utiliza: A)DR/BDR B)Root/Alternate C)PE/CE D)RR
25. Area 0 é: A)backbone B)externa C)gestão D)VLAN
26. LSA Type 1: A)Router LSA B)Network LSA C)externa D)ASBR
27. LSA Type 2: A)Network LSA associada ao DR B)BGP C)MPLS D)DHCP
28. OSPF preso em ExStart: causa comum? A)MTU incompatível B)DNS C)VLAN de usuário D)MPLS
29. Comando para vizinhos: A)show ip ospf neighbor B)show vlan brief C)show ip bgp D)show mpls
30. OSPF 2-Way em Ethernet pode ser: A)normal entre roteadores não selecionados para Full B)sempre erro C)MTU obrigatória D)falha BGP

## BGP
31. BGP utiliza: A)TCP 179 B)UDP 179 C)TCP 22 D)UDP 161
32. eBGP normalmente conecta: A)AS diferentes B)hosts C)mesmo AS D)switches L2
33. Local Preference influencia: A)saída do AS B)STP C)ARP D)DNS
34. AS_PATH ajuda a: A)identificar caminho por AS e evitar loops B)criar VLAN C)DNS D)labels
35. Na seleção Cisco tradicional, entre estes, maior precedência: A)Weight B)MED C)AS_PATH D)Origin
36. Prefix-list serve para: A)filtrar prefixos B)OSPF C)VLAN D)CPU
37. BGP Established mas prefixo não chega: A)investigar anúncio, AFI/SAFI, filtros, políticas e next-hop B)reiniciar tudo C)STP D)IPv6
38. Route Reflector: A)reduz necessidade de full mesh iBGP B)substitui OSPF C)NAT D)VLAN
39. Resumo de sessões: A)show ip bgp summary B)show vlan brief C)show ip ospf neighbor D)show mpls
40. Communities BGP: A)marcação/classificação e políticas B)MAC C)STP D)interfaces
41. Next-hop inacessível: A)rota pode não ser instalada/utilizada B)sempre vence C)vira VLAN D)vira ARP
42. iBGP: A)BGP dentro do mesmo AS B)entre VLANs C)somente MPLS D)somente IPv6
43. Maior Local Preference, em condições comparáveis: A)preferido para saída B)rejeitado C)STP D)IPv6
44. MED é usado para: A)influenciar entrada em AS vizinho conforme política B)Root Bridge C)VRF D)DNS
45. Examinar atributos de prefixo: A)show ip bgp <prefixo> B)show vlan brief C)show ip ospf interface D)ip neigh

## IPv6
46. IPv6 possui: A)32 B)64 C)128 D)256 bits
47. Link-local: A)FE80::/10 B)FC00::/7 C)FF00::/8 D)2000::/3
48. NDP usa: A)ICMPv6 B)ARP C)TCP D)UDP
49. SLAAC depende principalmente de: A)Router Advertisements B)DHCPv4 C)BGP D)STP
50. Multicast IPv6: A)FE80 B)FF00 C)FC00 D)2001
51. ::1 é: A)loopback B)multicast C)link-local D)gateway
52. NS/NA pertencem ao: A)NDP B)DNS C)BGP D)MPLS
53. Bloquear todo ICMPv6 pode: A)quebrar funções importantes B)melhorar sempre C)eliminar BGP D)eliminar VLAN

## MPLS e VRF
54. MPLS encaminha por: A)labels B)MAC somente C)DNS D)TCP ports
55. LSR: A)Label Switching Router B)Local Service Route C)Link Security Router D)LAN Switching Relay
56. LDP: A)distribuição de labels B)DNS C)DHCP D)STP
57. VRF permite: A)tabelas de roteamento separadas B)VLAN somente C)NAT somente D)IPv6 somente
58. PE em L3VPN: A)Provider Edge B)Private Ethernet C)Protocol Endpoint D)Packet Engine
59. CE conecta: A)cliente ao PE B)PE ao P C)switch ao Root D)BGP ao DNS
60. MP-BGP pode carregar: A)rotas VPNv4/VPNv6 B)MAC C)STP D)ARP
61. RD serve para: A)tornar rotas VPN distinguíveis B)senha C)Root Bridge D)MTU
62. RT serve para: A)controlar import/export de rotas VPN B)MAC C)VLAN nativa D)TCP
63. Rota correta na VRF, mas tráfego não passa: A)verificar LDP/LSP, labels, MP-BGP e encaminhamento B)DNS C)STP D)remover VRF

## Linux e monitoramento
64. Endereços IP Linux: A)ip addr B)show ip route C)route-map D)BGP
65. Tabela de rotas: A)ip route B)ip neigh C)ss D)dig
66. Vizinhos ARP/NDP: A)ip neigh B)ip route C)ss D)curl
67. Sockets TCP/UDP: A)ss -lntup B)ip addr C)dig D)traceroute
68. Captura de pacotes: A)tcpdump B)systemctl C)journalctl D)curl
69. Consulta DNS: A)dig B)mtr C)ss D)ip link
70. ip route get 8.8.8.8 mostra: A)como o Linux encaminhará B)MAC do DNS C)senha SSH D)VLAN
71. SNMP consultas: A)UDP 161 B)TCP 179 C)UDP 53 D)TCP 22
72. SNMP traps: A)UDP 162 B)TCP 443 C)UDP 67 D)TCP 25
73. SNMPv3 oferece: A)autenticação e privacidade B)BGP C)STP D)MPLS
74. Syslog: A)registro de eventos B)roteamento C)DHCP D)NAT
75. MTTR: A)Mean Time To Repair/Restore B)Maximum Traffic Transfer Rate C)Mean TCP Transfer Route D)Minimum Time To Route
76. MTBF: A)tempo médio entre falhas B)backup C)BGP D)broadcast

## Troubleshooting
77. Ping gateway funciona, aplicação TCP não: A)verificar porta/serviço/firewall B)trocar IP C)STP global D)BGP global
78. Perda só no pico: A)congestionamento/filas/drops B)DNS C)Router ID D)VLAN
79. Muitos CRC/errors: A)investigar físico/cabo/óptica/duplex B)BGP C)DNS D)VRF
80. Um hop do traceroute tem latência alta, seguintes normais: A)pode ser rate-limit de ICMP; não concluir falha B)hop quebrado C)DNS D)VLAN
81. Antes de mudar configuração em incidente: A)coletar evidências e avaliar impacto B)reiniciar tudo C)apagar logs D)trocar cabos
82. Loop L2 causa: A)broadcast storm/instabilidade MAC B)DNS C)BGP Established D)menor utilização
83. MAC flapping indica: A)possível loop ou problema L2 redundante B)DNS C)NTP D)BGP obrigatório
84. Primeiro passo: A)definir sintoma, escopo e impacto B)mudar configuração C)reiniciar D)desabilitar firewall
85. Confirmar rota no RIB Cisco: A)show ip route B)show vlan brief C)show logging D)show users
86. Verificar interface: A)show ip interface brief B)show ip bgp C)show ip ospf database D)show mpls
87. Evidência serve para: A)reduzir hipóteses e orientar testes B)ser ignorada C)vir depois da mudança D)ser substituída por opinião
88. Apenas um prefixo BGP desapareceu: A)comparar anúncio, filtros, políticas e RIB/FIB B)reiniciar AS C)STP D)IPv6
89. Todos os serviços atrás de uma VLAN falham: A)verificar VLAN, trunk, SVI, ARP, DHCP e rota B)BGP externo C)DNS público D)MPLS
90. DNS resolve, TCP connection refused: A)host respondeu, mas serviço/porta pode não aceitar B)DNS quebrado C)IP inexistente D)ARP necessariamente falhou
91. ping funciona, curl falha: A)investigar TCP, porta, TLS, HTTP e firewall B)rede perfeita C)STP D)DNS
92. Rota mais específica: A)normalmente vence menos específica B)perde C)ignorada D)vira default
93. Alta utilização sem erros físicos: A)pode haver congestionamento B)cabo rompido C)DNS D)Router ID
94. MTU incompatível pode causar: A)drops/falhas seletivas/PMTUD B)sempre BGP Down C)STP loop D)DNS
95. Assimetria pode: A)complicar troubleshooting e firewalls stateful B)melhorar sempre C)eliminar ARP D)criar VLAN
96. Incidente deve separar: A)sintoma, causa raiz e impacto B)configuração C)logs D)opinião
97. Boa mudança tem: A)plano, risco, rollback e validação B)apenas comando C)nenhum registro D)sem janela
98. Reduz MTTR: A)runbooks, observabilidade, automação e procedimentos claros B)remover monitoramento C)mais mudanças manuais D)ocultar incidentes
99. Após correção: A)validar, monitorar e documentar B)encerrar sem evidência C)apagar logs D)ignorar recorrência
100. Postura sênior: A)evidências, impacto, risco e validação B)mudanças aleatórias C)opinião D)reiniciar tudo

## Gabarito
1-B 2-B 3-C 4-B 5-A 6-B 7-B 8-A 9-B 10-B
11-A 12-A 13-A 14-A 15-A 16-A 17-A 18-A 19-A 20-B
21-A 22-A 23-C 24-A 25-A 26-A 27-A 28-A 29-A 30-A
31-A 32-A 33-A 34-A 35-A 36-A 37-A 38-A 39-A 40-A
41-A 42-A 43-A 44-A 45-A 46-C 47-A 48-A 49-A 50-B
51-A 52-A 53-A 54-A 55-A 56-A 57-A 58-A 59-A 60-A
61-A 62-A 63-A 64-A 65-A 66-A 67-A 68-A 69-A 70-A
71-A 72-A 73-A 74-A 75-A 76-A 77-A 78-A 79-A 80-A
81-A 82-A 83-A 84-A 85-A 86-A 87-A 88-A 89-A 90-A
91-A 92-A 93-A 94-A 95-A 96-A 97-A 98-A 99-A 100-A

## Como usar o banco

### Rodada 1 — velocidade
Responda as 100 sem consultar. Marque apenas as que geraram dúvida.

### Rodada 2 — justificativa
Para cada erro, escreva em uma linha **por que a alternativa correta é correta e por que a sua estava errada**.

### Rodada 3 — foco RNP
Priorize:
- subnetting/CIDR/LPM;
- OSPF states, LSAs, DR/BDR e ExStart;
- BGP best-path, políticas e next-hop;
- VLAN/trunk/STP;
- IPv6/NDP/ICMPv6;
- MPLS/VRF/RD/RT/MP-BGP;
- Linux iproute2, sockets, DNS e tcpdump;
- troubleshooting baseado em evidências.

## Pegadinhas para revisar

1. **/27 = 30 hosts utilizáveis**, não 32.
2. **Longest Prefix Match** escolhe o prefixo mais específico.
3. **OSPF protocol 89**; não usa TCP/179.
4. **OSPF 2-Way pode ser normal** em Ethernet entre DROTHERs.
5. **OSPF ExStart → MTU** é uma verificação clássica.
6. **BGP Established não garante prefixo na RIB/FIB.**
7. **Weight** tem alta precedência na seleção Cisco tradicional.
8. **RD distingue; RT controla import/export.**
9. **NDP usa ICMPv6**, não ARP.
10. Bloquear indiscriminadamente **ICMPv6** pode quebrar Neighbor Discovery e PMTUD.
11. Um hop lento no traceroute **não é automaticamente a causa**.
12. CRC/errors apontam primeiro para investigação física/L2.
13. `LISTEN` no Linux não prova que a aplicação está saudável.
14. DNS resolvido não prova que TCP/443 funciona.
15. Em troubleshooting, **evidência vem antes da mudança**.

## Meta de desempenho

| Resultado | Interpretação |
|---|---|
| 90–100 | Excelente — revisar apenas pegadinhas |
| 80–89 | Bom — reforçar pontos de erro |
| 70–79 | Atenção — revisar fundamentos e cenários |
| <70 | Fazer nova rodada antes do simulado final |
