# 01 — Fundamentos de Redes

## 1. Visão geral

Fundamentos são a base para entender **switching, routing, troubleshooting, segurança e performance**. Em uma prova de Analista de Redes Sênior, o mais importante não é apenas decorar protocolos, mas entender o caminho de um pacote e saber localizar onde a comunicação falha.

### Fluxo mental

**Aplicação → TCP/UDP → IP → Ethernet → meio físico**

Na resposta de troubleshooting, pense no sentido inverso:

**Físico → L2 → L3 → Transporte → Aplicação**

---

## 2. Modelo OSI

| Camada | Nome | Exemplos | Pergunta de troubleshooting |
|---|---|---|---|
| 7 | Aplicação | DNS, HTTP, SSH, SNMP | O serviço responde? |
| 6 | Apresentação | TLS, codificação | Há problema de criptografia/formato? |
| 5 | Sessão | controle de sessão | A sessão permanece ativa? |
| 4 | Transporte | TCP, UDP | A porta está acessível? |
| 3 | Rede | IPv4, IPv6, ICMP | Existe rota? |
| 2 | Enlace | Ethernet, VLAN, STP | MAC/VLAN estão corretos? |
| 1 | Física | fibra, cobre, rádio | Link está ativo e sem erros? |

### Decore para a prova

- **L1:** sinal e meio físico.
- **L2:** MAC, Ethernet, VLAN, STP.
- **L3:** IP, ICMP, routing.
- **L4:** TCP/UDP e portas.
- **L7:** aplicações e serviços.

---

## 3. Modelo TCP/IP

O modelo TCP/IP pode ser simplificado em:

1. **Acesso à rede** — Ethernet/Wi-Fi.
2. **Internet** — IPv4/IPv6/ICMP.
3. **Transporte** — TCP/UDP.
4. **Aplicação** — DNS, HTTP, SSH, SNMP etc.

### Encapsulamento

Quando um host envia dados:

**Dados → segmento/datagrama → pacote IP → frame Ethernet → bits**

No destino ocorre o processo inverso.

---

## 4. Ethernet e MAC

Ethernet é a principal tecnologia de comunicação em redes locais.

### Conceitos importantes

- MAC identifica uma interface no domínio L2.
- Switch aprende o MAC pela **MAC de origem** recebida.
- O switch encaminha unicast pela porta conhecida.
- Broadcast é encaminhado dentro do domínio de broadcast.
- Um roteador normalmente separa domínios de broadcast.
- VLANs permitem separar logicamente domínios de broadcast.

### Tabela MAC

Um switch aprende:

**MAC de origem → porta de entrada → VLAN**

Comando Cisco:

`show mac address-table`

---

## 5. ARP

ARP resolve:

**IPv4 → MAC**

Exemplo:

Um host quer enviar para 192.168.1.1. Se não conhece o MAC, utiliza ARP para descobrir o endereço L2 correspondente.

Comandos:

`arp -a`

`ip neigh`

### Atenção

ARP funciona no contexto IPv4. No IPv6, o mecanismo equivalente faz parte do **NDP/ICMPv6**.

---

## 6. ICMP

ICMP é usado para mensagens de controle e diagnóstico.

Comandos:

`ping`

`traceroute`

`tracert`

### Troubleshooting

- Ping ao gateway funciona → L1/L2/L3 local provavelmente estão funcionando.
- Ping funciona, mas TCP falha → investigar porta, firewall e serviço.
- Traceroute mostra um hop com latência alta, mas os seguintes normalizam → não conclua automaticamente que aquele roteador é a causa.

---

## 7. IPv4

IPv4 possui **32 bits** e é representado em quatro octetos.

Exemplo:

`192.168.10.25/24`

A máscara/prefixo determina:

- parte de rede;
- parte de host;
- quantidade de endereços;
- tamanho do domínio de endereçamento.

### Endereços privados

- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16

### Conceitos que caem em prova

- CIDR
- subnetting
- VLSM
- sumarização
- rota default
- longest prefix match
- endereço de rede
- broadcast
- primeiro/último host

---

## 8. Routing — conceito fundamental

Quando um roteador recebe um pacote, ele consulta a tabela de roteamento e escolhe a rota mais específica aplicável.

### Longest Prefix Match

Exemplo:

- 10.0.0.0/8
- 10.10.0.0/16
- 10.10.10.0/24

Para o destino **10.10.10.50**, a rota /24 é mais específica e normalmente será escolhida.

### Comandos Cisco

`show ip route`

`show ip route <destino>`

`show ip interface brief`

### Linux

`ip route`

`ip route get <destino>`

---

## 9. TCP x UDP

### TCP

- Orientado à conexão.
- Confiável.
- Usa confirmação e retransmissão.
- Possui controle de fluxo.
- Possui controle de congestionamento.
- Usa handshake para estabelecimento da conexão.

### UDP

- Não orientado à conexão.
- Menor overhead.
- Não garante entrega.
- Não possui retransmissão nativa equivalente ao TCP.

### TCP — handshake

**SYN → SYN/ACK → ACK**

No encerramento, normalmente são utilizados FIN/ACK.

---

## 10. MTU

MTU é o tamanho máximo do pacote/frame IP que pode ser transportado sem fragmentação no contexto considerado.

Problemas de MTU podem causar:

- perda seletiva;
- conexões que iniciam e depois falham;
- problemas com PMTUD;
- comportamento diferente entre ping e aplicações.

Comandos úteis:

`ping` com tamanho controlado

`tracepath`

`tcpdump`

### Regra de troubleshooting

Se pequenos pacotes funcionam e aplicações maiores falham, **MTU/PMTUD deve entrar na lista de hipóteses**.

---

## 11. DNS

DNS traduz nomes em informações de resolução, principalmente nomes para endereços IP.

Comandos:

`dig exemplo.com`

`nslookup exemplo.com`

### Diferencie

**DNS resolve → IP funciona → TCP pode estar bloqueado → aplicação pode estar indisponível.**

Portanto, DNS funcionando não significa que a aplicação esteja funcionando.

---

## 12. DHCP

DHCP permite distribuição automática de parâmetros de rede.

O processo clássico IPv4 é:

**DORA**

1. Discover
2. Offer
3. Request
4. Acknowledge

Parâmetros comuns:

- endereço IP;
- máscara;
- gateway;
- DNS;
- tempo de concessão.

---

## 13. Portas importantes

| Porta | Serviço |
|---:|---|
| 22/TCP | SSH |
| 23/TCP | Telnet |
| 53/TCP/UDP | DNS |
| 67/UDP | DHCP Server |
| 68/UDP | DHCP Client |
| 80/TCP | HTTP |
| 123/UDP | NTP |
| 161/UDP | SNMP |
| 162/UDP | SNMP Trap |
| 443/TCP | HTTPS |
| 514/UDP/TCP | Syslog, conforme implementação |

### Muito importante

**BGP = TCP/179**

**OSPF = IP protocol 89, não TCP/UDP**

**ICMP = IP protocol 1 no IPv4**

---

## 14. Troubleshooting por camadas

### L1 — Física
Verifique:

- interface up/down;
- cabo/fibra;
- potência óptica;
- CRC;
- errors;
- flaps;
- velocidade/duplex.

### L2 — Enlace
Verifique:

- VLAN;
- access/trunk;
- MAC table;
- STP;
- EtherChannel;
- broadcast/loop.

### L3 — Rede
Verifique:

- IP/máscara;
- ARP;
- gateway;
- tabela de rotas;
- ACL;
- VRF;
- IPv4/IPv6.

### L4 — Transporte
Verifique:

- porta TCP/UDP;
- firewall;
- socket;
- timeout;
- retransmissões.

### L7 — Aplicação
Verifique:

- DNS;
- TLS;
- HTTP;
- autenticação;
- disponibilidade do serviço.

---

## 15. Comandos essenciais

### Cisco

`show ip interface brief`

`show interfaces`

`show ip route`

`show ip arp`

`show mac address-table`

`show vlan brief`

`show interfaces trunk`

`show logging`

### Linux

`ip addr`

`ip link`

`ip route`

`ip neigh`

`ss -lntup`

`ping`

`traceroute`

`mtr`

`dig`

`curl -I`

`tcpdump`

---

## 16. Checklist rápido para a prova

Antes de responder uma questão de troubleshooting, pergunte:

1. **Qual é o sintoma?**
2. **Qual é o escopo?** Um host, uma VLAN, um POP ou toda a rede?
3. **Qual camada pode estar envolvida?**
4. **Que evidência tenho?**
5. **Qual hipótese é mais provável?**
6. **Qual teste é menos invasivo?**
7. **Como vou validar a correção?**

### Regra de ouro

**Sintoma → evidência → hipótese → teste → causa raiz → correção → validação → prevenção.**

---

## 17. Pegadinhas frequentes

- **OSPF não usa TCP/UDP.** Usa IP protocol 89.
- **BGP usa TCP/179.**
- **ARP é IPv4; NDP é IPv6.**
- **STP resolve loops de camada 2.**
- **VLAN separa domínios de broadcast.**
- **Ping funcionar não significa aplicação funcionando.**
- **DNS resolver não significa serviço TCP disponível.**
- **Um hop lento no traceroute não prova problema naquele hop.**
- **Rota mais específica normalmente vence rota menos específica.**
- **Antes de mudar configuração, colete evidências.**

## Objetivo para o nível Sênior

Não basta responder **"qual protocolo?"**.

Esteja preparado para explicar:

> **O que está acontecendo → como eu comprovo → qual comando uso → qual resultado espero → qual ação tomo → como valido.**
