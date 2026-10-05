# IPv6 — Endereçamento, NDP, SLAAC, Roteamento e Troubleshooting

## 1. Visão geral

IPv6 foi projetado para substituir o IPv4 e possui **128 bits**, permitindo um espaço de endereçamento muito maior.

### Principais características

- Endereços de 128 bits.
- Notação hexadecimal.
- Não utiliza broadcast.
- Utiliza **multicast** e **anycast** para diversos mecanismos.
- NDP (**Neighbor Discovery Protocol**) substitui funções importantes do ARP.
- ICMPv6 é parte fundamental do funcionamento do protocolo.
- Suporta autoconfiguração de hosts.
- Cabeçalho IPv6 possui estrutura mais simples que o IPv4.
- Fragmentação é realizada pelo host de origem, não pelos roteadores intermediários.
- Roteadores IPv6 utilizam o **Hop Limit**, equivalente ao TTL do IPv4.

### Mentalidade para a prova

```text
Endereço → Prefixo → Neighbor Discovery → Gateway → Rota → ICMPv6 → Serviço
```

---

# 2. Notação IPv6

Um endereço IPv6 possui oito grupos de 16 bits:

```text
2001:0db8:0001:0000:0000:0000:0000:0001
```

Pode ser reduzido:

```text
2001:db8:1::1
```

## Regras de abreviação

### Remover zeros à esquerda

```text
2001:0db8:0001:0000
→
2001:db8:1:0
```

### Utilizar `::`

`::` representa uma sequência de grupos de zeros.

Exemplo:

```text
2001:db8:0:0:0:0:0:1
→
2001:db8::1
```

**Pegadinha:** `::` pode aparecer apenas **uma vez** no mesmo endereço.

---

# 3. Principais tipos de endereço

| Tipo | Prefixo | Função |
|---|---|---|
| Global Unicast | normalmente `2000::/3` | Endereço globalmente roteável |
| Link-Local | `FE80::/10` | Comunicação no enlace local |
| Unique Local | `FC00::/7` | Uso privado/local |
| Multicast | `FF00::/8` | Comunicação para grupo |
| Loopback | `::1/128` | Próprio host |
| Unspecified | `::/128` | Endereço não especificado |

## Link-Local

Todo dispositivo IPv6 utiliza endereços link-local para funções locais do enlace.

Exemplo:

```text
FE80::1
```

O endereço link-local:

- não é roteável entre enlaces;
- é utilizado pelo NDP;
- pode ser utilizado como next-hop em protocolos de roteamento;
- é muito importante em OSPFv3 e outros mecanismos IPv6.

**Pegadinha de prova:** `FE80::/10` não significa endereço global.

---

# 4. Prefixo e subnetting

IPv6 utiliza CIDR.

Exemplo:

```text
2001:db8:100:10::/64
```

Em redes LAN IPv6, **/64 é o prefixo tradicional mais comum**.

Exemplo:

```text
Prefixo:
2001:db8:100:10::/64

Host:
2001:db8:100:10::1234
```

Diferentemente do IPv4, não devemos tratar o endereço IPv6 simplesmente como "rede + host" usando as mesmas premissas de subnetting.

---

# 5. NDP — Neighbor Discovery Protocol

O **NDP** é baseado em **ICMPv6** e substitui/expande funções que no IPv4 são realizadas por ARP, ICMP Router Discovery e mecanismos relacionados.

Principais funções:

- descoberta de vizinhos;
- resolução de endereço de camada 2;
- descoberta de roteadores;
- autoconfiguração;
- detecção de endereços duplicados;
- descoberta de parâmetros do enlace;
- redirecionamento.

## Principais mensagens NDP

| Mensagem | Sigla | Função |
|---|---|---|
| Router Solicitation | RS | Host solicita informações ao roteador |
| Router Advertisement | RA | Roteador anuncia informações/prefixos |
| Neighbor Solicitation | NS | Descoberta/verificação de vizinho |
| Neighbor Advertisement | NA | Resposta à NS |
| Redirect | Redirect | Informa caminho/next-hop mais apropriado |

### Fluxo típico

```text
Host
  |
  | RS
  v
Router
  |
  | RA
  v
Host
```

O RA pode fornecer informações como:

- prefixo;
- gateway/default router;
- parâmetros de configuração;
- informações utilizadas pelo SLAAC.

---

# 6. NDP x ARP

No IPv4:

```text
ARP → IPv4 para MAC
```

No IPv6:

```text
NDP/ICMPv6 → descoberta de vizinhos e resolução de camada 2
```

**IPv6 não utiliza ARP.**

Isso é uma pegadinha clássica de prova.

---

# 7. Multicast no IPv6

Como IPv6 não possui broadcast, vários mecanismos utilizam multicast.

Exemplos importantes:

```text
FF02::1     → All Nodes
FF02::2     → All Routers
FF02::5     → OSPFv3 AllSPF
FF02::6     → OSPFv3 AllDRouters
```

### Solicited-Node Multicast

É utilizado pelo NDP para descobrir/verificar vizinhos.

É associado aos endereços IPv6 das interfaces e permite substituir a função de descoberta que no IPv4 utiliza broadcast ARP.

**Pegadinha:** IPv6 não faz ARP broadcast.

---

# 8. SLAAC — Stateless Address Autoconfiguration

SLAAC permite que o host configure seu endereço IPv6 automaticamente utilizando informações anunciadas pelo roteador.

Fluxo simplificado:

```text
1. Host inicia
2. Host gera/possui Link-Local
3. DAD pode verificar duplicidade
4. Host envia RS
5. Router responde RA
6. Host aprende prefixo
7. Host forma endereço IPv6
8. Host aprende default router
```

## DAD — Duplicate Address Detection

Antes de utilizar um endereço, o host pode verificar se outro dispositivo já utiliza aquele endereço.

O processo utiliza **Neighbor Solicitation**.

**Pegadinha:** DAD não utiliza ARP.

---

# 9. SLAAC x DHCPv6

| Característica | SLAAC | DHCPv6 |
|---|---|---|
| Configuração automática | Sim | Sim |
| Prefixo via RA | Sim | Não é substituído pelo DHCPv6 |
| Default Router | RA | RA |
| Informações adicionais | Limitadas conforme RA | Pode fornecer informações adicionais |
| Estado | Stateless | Pode ser stateful ou stateless |

### Importante

O **RA continua importante mesmo quando DHCPv6 é utilizado**, especialmente para descoberta do default router.

---

# 10. Flags importantes do RA

Os Router Advertisements possuem flags que orientam o comportamento do host.

### A — Autonomous

Indica que o prefixo pode ser utilizado para autoconfiguração via SLAAC.

### M — Managed

Indica utilização de configuração stateful via DHCPv6.

### O — Other

Indica que outras informações podem ser obtidas via DHCPv6.

**Pegadinha:** DHCPv6 não elimina a necessidade do RA para descoberta do roteador padrão.

---

# 11. ICMPv6

ICMPv6 é essencial para o funcionamento do IPv6.

É utilizado para:

- erros;
- diagnóstico;
- descoberta de vizinhos;
- descoberta de roteadores;
- Path MTU Discovery;
- NDP;
- controle do protocolo.

## Mensagens importantes

```text
Type 1  → Destination Unreachable
Type 2  → Packet Too Big
Type 3  → Time Exceeded
Type 4  → Parameter Problem
Type 128 → Echo Request
Type 129 → Echo Reply
```

### Packet Too Big

É especialmente importante porque os roteadores IPv6 não fragmentam pacotes em trânsito.

Se o pacote exceder o MTU do próximo enlace, o roteador envia:

```text
ICMPv6 Packet Too Big
```

O host de origem ajusta o tamanho dos pacotes.

---

# 12. Path MTU Discovery

Em IPv6:

```text
Host origem
     |
     | pacote
     v
Router
     |
     | MTU insuficiente
     v
ICMPv6 Packet Too Big
     |
     v
Host reduz tamanho
```

**Pegadinha de prova:** bloquear ICMPv6 indiscriminadamente pode causar problemas de conectividade, inclusive relacionados ao PMTUD.

---

# 13. Fragmentação

No IPv4, roteadores podem fragmentar pacotes.

No IPv6:

**roteadores não fragmentam pacotes em trânsito.**

A fragmentação, quando necessária, é feita pelo host de origem utilizando o Fragment Extension Header.

Isso torna o PMTUD especialmente importante.

---

# 14. Roteamento IPv6

O IPv6 pode utilizar protocolos como:

- OSPFv3;
- MP-BGP;
- IS-IS;
- RIPng.

### OSPFv3

É utilizado para roteamento interno IPv6.

Comandos Cisco:

```text
show ipv6 ospf neighbor
show ipv6 ospf interface
show ipv6 ospf database
show ipv6 route ospf
```

### BGP IPv6

BGP pode transportar IPv6 através da Address Family correspondente.

Exemplo conceitual:

```text
BGP
 └── AFI IPv6
      └── SAFI Unicast
```

Em ambientes Cisco, normalmente é necessário verificar se a address-family IPv6 está ativada para o vizinho.

---

# 15. IPv6 e camada 2

Para troubleshooting de uma LAN IPv6:

```text
Interface
   ↓
VLAN
   ↓
IPv6 address
   ↓
Link-Local
   ↓
NDP
   ↓
RA/SLAAC
   ↓
Default Router
   ↓
IPv6 Route
   ↓
ICMPv6
   ↓
Aplicação
```

---

# 16. Troubleshooting IPv6 — método Sênior

Quando um host IPv6 não consegue acessar outro destino:

## 1. Verificar interface

```text
show ipv6 interface
show ipv6 interface brief
```

Perguntas:

- Interface está up/up?
- IPv6 está habilitado?
- Existe endereço global?
- Existe link-local?

## 2. Verificar vizinhos

```text
show ipv6 neighbors
```

Verifique:

- endereço IPv6;
- endereço MAC;
- interface;
- estado do vizinho.

## 3. Verificar rotas

```text
show ipv6 route
show ipv6 route <prefixo>
```

Perguntas:

- Existe rota?
- Existe default route?
- Qual é o next-hop?
- A rota está ativa?

## 4. Testar ICMPv6

```text
ping ipv6 <endereco>
```

## 5. Testar caminho

```text
traceroute ipv6 <endereco>
```

## 6. Verificar MTU

Sintomas de MTU:

- ping pequeno funciona;
- aplicações maiores falham;
- TCP apresenta comportamento estranho;
- conexões ficam parciais;
- transferência trava.

Investigue:

```text
MTU → PMTUD → ICMPv6 Packet Too Big → firewall
```

## 7. Verificar firewall/ACL

Não bloqueie ICMPv6 indiscriminadamente.

Verifique especialmente:

- NDP;
- RS/RA;
- NS/NA;
- Packet Too Big;
- Echo Request/Reply;
- mensagens ICMPv6 necessárias ao ambiente.

---

# 17. Troubleshooting: Host possui IPv6, mas não navega

### Sintoma

```text
Host:
2001:db8:10:10::100/64

Consegue acessar o próprio endereço,
mas não consegue acessar a Internet IPv6.
```

### Sequência de investigação

```text
1. Interface
2. Link-local
3. Default Router
4. NDP
5. RA
6. Default Route
7. Rota no upstream
8. BGP/OSPFv3
9. Firewall
10. PMTUD
```

Não conclua imediatamente que "IPv6 está configurado".

Ter um endereço IPv6 **não significa** que existe conectividade IPv6 fim a fim.

---

# 18. Troubleshooting: ping funciona, aplicação não

Se:

```text
ping IPv6 → OK
TCP/443 → falha
```

investigue:

- DNS;
- endereço AAAA;
- firewall;
- ACL;
- TCP/443;
- MTU/PMTUD;
- aplicação;
- preferência IPv4/IPv6;
- rota de retorno.

**Regra:** ICMPv6 funcionando não prova que a aplicação está funcionando.

---

# 19. DNS e IPv6

Registros DNS importantes:

```text
A    → IPv4
AAAA → IPv6
```

Exemplo:

```text
www.exemplo.com
    AAAA
    2001:db8::100
```

Comandos Linux:

```text
dig AAAA exemplo.com
nslookup -type=AAAA exemplo.com
```

---

# 20. Segurança IPv6

IPv6 não significa "mais seguro automaticamente".

Deve-se considerar:

- ACL IPv6;
- firewall;
- RA Guard;
- DHCPv6 Guard;
- proteção contra ataques NDP;
- controle de Router Advertisement;
- segmentação;
- IPsec quando aplicável;
- monitoramento.

### RA spoofing

Um atacante pode tentar enviar RA falso para induzir hosts a utilizar um gateway malicioso.

Em redes corporativas/switching, mecanismos como **RA Guard** podem ajudar a limitar anúncios de roteadores não autorizados.

---

# 21. IPv4 x IPv6 — comparação para prova

| Característica | IPv4 | IPv6 |
|---|---|---|
| Tamanho | 32 bits | 128 bits |
| Broadcast | Sim | Não |
| ARP | Sim | Não |
| Descoberta de vizinhos | ARP | NDP/ICMPv6 |
| Configuração automática | DHCP | SLAAC/DHCPv6 |
| TTL | TTL | Hop Limit |
| Fragmentação por roteador | Pode ocorrer | Não |
| Multicast | Sim | Muito utilizado |
| PMTUD | Importante | Muito importante |
| ICMP | ICMP | ICMPv6 |

---

# 22. Comandos Cisco — Quick Reference

### Interface

```text
show ipv6 interface
show ipv6 interface brief
```

### Endereços e vizinhos

```text
show ipv6 neighbors
```

### Roteamento

```text
show ipv6 route
show ipv6 route connected
show ipv6 route local
show ipv6 route ospf
```

### Testes

```text
ping ipv6 <endereco>
traceroute ipv6 <endereco>
```

### OSPFv3

```text
show ipv6 ospf neighbor
show ipv6 ospf interface
show ipv6 ospf database
```

### BGP

Em plataformas que utilizam address-family IPv6:

```text
show bgp ipv6 unicast summary
show bgp ipv6 unicast
show bgp ipv6 unicast <prefixo>
```

---

# 23. Comandos Linux

### Endereços

```bash
ip -6 addr
ip -6 addr show dev eth0
```

### Rotas

```bash
ip -6 route
ip -6 route get 2001:db8::1
```

### Vizinhos

```bash
ip -6 neigh
```

### Testes

```bash
ping -6 2001:db8::1
traceroute -6 2001:db8::1
```

### DNS

```bash
dig AAAA exemplo.com
```

### Captura

```bash
tcpdump -ni eth0 ip6
tcpdump -ni eth0 icmp6
```

---

# 24. Cenário de prova — NDP

### Problema

Dois hosts estão na mesma VLAN IPv6, mas não conseguem se comunicar.

### Investigação

```text
1. Interface up?
2. Mesmo prefixo?
3. Link-local presente?
4. NDP possui vizinho?
5. NS está sendo enviado?
6. NA está retornando?
7. MAC aparece na tabela?
8. VLAN/trunk está correto?
9. ACL/firewall bloqueia ICMPv6?
```

### Resposta Sênior

Não começaria alterando configuração. Primeiro verificaria interface, endereço/prefixo, tabela de vizinhos, captura de ICMPv6 e camada 2. Depois validaria ACL/firewall e somente então aplicaria a correção.

---

# 25. Cenário de prova — RA não chega ao host

### Sintoma

Host possui apenas endereço link-local e não recebe endereço global via SLAAC.

### Hipóteses

- RA desabilitado;
- interface IPv6 incorreta;
- VLAN incorreta;
- switch bloqueando RA;
- RA Guard mal configurado;
- problema de NDP/ICMPv6;
- firewall/ACL;
- roteador não está anunciando o prefixo;
- prefixo do RA configurado incorretamente.

### Sequência

```text
Host
 ↓
ICMPv6 RS
 ↓
Switch/VLAN
 ↓
Router
 ↓
ICMPv6 RA
 ↓
Prefixo
 ↓
SLAAC
```

---

# 26. Pegadinhas da prova RNP

1. IPv6 possui **128 bits**.
2. IPv6 não utiliza ARP.
3. IPv6 não possui broadcast.
4. NDP utiliza ICMPv6.
5. Link-local é `FE80::/10`.
6. Multicast começa com `FF00::/8`.
7. Loopback IPv6 é `::1`.
8. Unspecified é `::`.
9. SLAAC depende de informações recebidas via RA.
10. DHCPv6 não substitui a descoberta do default router via RA.
11. DAD utiliza NDP/ICMPv6.
12. IPv6 não permite fragmentação por roteadores intermediários.
13. PMTUD é muito importante em IPv6.
14. ICMPv6 não deve ser bloqueado indiscriminadamente.
15. Ter endereço IPv6 não significa ter conectividade fim a fim.
16. Ping funcionando não garante que TCP/443 funcionará.
17. NDP é muito mais que "ARP do IPv6"; também participa de descoberta de roteadores e outros mecanismos.
18. Link-local pode ser utilizado como next-hop em protocolos de roteamento.
19. IPv6 utiliza Hop Limit em vez de TTL.
20. RA pode informar prefixo e parâmetros importantes para o host.

---

# 27. Resumo de 30 segundos

> **IPv6 possui 128 bits, não usa broadcast nem ARP. NDP, baseado em ICMPv6, realiza descoberta de vizinhos e roteadores. RS solicita informações e RA anuncia prefixos e parâmetros. SLAAC permite autoconfiguração. Link-local usa FE80::/10, multicast usa FF00::/8 e loopback é ::1. IPv6 não é fragmentado por roteadores, portanto PMTUD e ICMPv6 são fundamentais. No troubleshooting, começo por interface, endereço, NDP, RA/default route, tabela IPv6, caminho, ACL/firewall e MTU.**

## Regra de ouro

```text
Sintoma
  ↓
Interface
  ↓
IPv6 Address / Prefix
  ↓
NDP / ICMPv6
  ↓
RA / Default Route
  ↓
Routing
  ↓
ACL / Firewall
  ↓
MTU / PMTUD
  ↓
Aplicação
  ↓
Validação
```
