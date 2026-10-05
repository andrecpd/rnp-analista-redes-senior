# IPv4 e Subnetting — Resumo para Prova

## 1. Conceito

IPv4 possui **32 bits**, divididos em parte de rede e parte de host pelo prefixo CIDR.

Exemplo:

`192.168.10.70/26`

O **/26** significa que 26 bits identificam a rede e 6 bits ficam disponíveis para hosts.

---

## 2. Prefixos mais importantes

| CIDR | Máscara | Hosts utilizáveis* | Tamanho do bloco |
|---|---|---:|---:|
| /24 | 255.255.255.0 | 254 | 256 |
| /25 | 255.255.255.128 | 126 | 128 |
| /26 | 255.255.255.192 | 62 | 64 |
| /27 | 255.255.255.224 | 30 | 32 |
| /28 | 255.255.255.240 | 14 | 16 |
| /29 | 255.255.255.248 | 6 | 8 |
| /30 | 255.255.255.252 | 2 | 4 |

*Regra tradicional para sub-redes IPv4: **2^bits de host − 2**.

---

## 3. Como calcular rapidamente

### Exemplo

**192.168.10.70/26**

### Passo 1 — Máscara

/26 = **255.255.255.192**

### Passo 2 — Tamanho do bloco

256 − 192 = **64**

As redes são:

- 192.168.10.0
- 192.168.10.64
- 192.168.10.128
- 192.168.10.192

### Passo 3 — Encontrar a rede

70 está entre 64 e 127.

Logo:

**Rede = 192.168.10.64/26**

### Passo 4 — Broadcast

Próxima rede − 1:

**192.168.10.127**

### Passo 5 — Hosts

- Primeiro: **192.168.10.65**
- Último: **192.168.10.126**

### Resultado

**Rede:** 192.168.10.64/26  
**Primeiro host:** 192.168.10.65  
**Último host:** 192.168.10.126  
**Broadcast:** 192.168.10.127  
**Hosts utilizáveis:** 62

---

## 4. VLSM

VLSM permite utilizar sub-redes de tamanhos diferentes conforme a necessidade.

Exemplo de planejamento:

- 100 hosts → /25
- 50 hosts → /26
- 20 hosts → /27
- 10 hosts → /28
- 2 hosts → /30

### Regra prática

Comece pela **maior necessidade** e depois aloque as menores.

Isso reduz desperdício de endereços.

---

## 5. Sumarização

A sumarização representa vários prefixos com uma rota mais abrangente.

Exemplo:

- 10.10.0.0/24
- 10.10.1.0/24
- 10.10.2.0/24
- 10.10.3.0/24

Podem ser sumarizados como:

**10.10.0.0/22**

Benefícios:

- menor tabela de rotas;
- menos atualizações;
- maior estabilidade;
- melhor escalabilidade.

---

## 6. Longest Prefix Match

Quando existem várias rotas possíveis, o roteador normalmente escolhe o **prefixo mais específico**.

Exemplo:

- 10.0.0.0/8
- 10.10.0.0/16
- 10.10.10.0/24

Para **10.10.10.50**, a rota **/24** é mais específica.

### Atenção

Não confunda:

**Longest Prefix Match** com **Administrative Distance** ou **métrica**.

O processo de seleção depende do contexto e da implementação, mas o princípio de especificidade é fundamental.

---

## 7. Endereços especiais

### Rede

Identifica a sub-rede.

### Broadcast

Em IPv4, representa o endereço de broadcast da sub-rede.

### Host

Endereços disponíveis para dispositivos.

### Default route

**0.0.0.0/0**

É a rota menos específica e pode ser utilizada quando não existe uma rota mais específica aplicável.

---

## 8. Faixas privadas

- **10.0.0.0/8**
- **172.16.0.0/12**
- **192.168.0.0/16**

São utilizadas em redes privadas e normalmente não são roteadas diretamente na Internet pública.

---

## 9. Troubleshooting de endereçamento

Quando dois hosts não se comunicam, valide:

1. IP;
2. máscara/prefixo;
3. gateway;
4. rede de destino;
5. ARP;
6. tabela de rotas;
7. ACL/firewall;
8. VLAN;
9. MTU;
10. aplicação/porta.

### Exemplo

Se:

**Host A:** 192.168.10.10/24  
**Host B:** 192.168.20.10/24

Eles estão em redes diferentes. Precisam de roteamento para comunicação entre as sub-redes.

---

## 10. Comandos para prova

### Cisco

`show ip interface brief`

`show ip route`

`show ip route <destino>`

`show ip arp`

`show running-config interface <interface>`

### Linux

`ip addr`

`ip route`

`ip route get <destino>`

`ip neigh`

`ping <destino>`

`traceroute <destino>`

---

## 11. Checklist de subnetting

Na prova, siga sempre:

**Prefixo → máscara → bloco → rede → broadcast → primeiro host → último host → quantidade de hosts**

---

## 12. Pegadinhas de prova

- /30 possui **2 hosts utilizáveis** na regra tradicional.
- /27 possui **30 hosts utilizáveis**.
- /26 possui **62 hosts utilizáveis**.
- **0.0.0.0/0** é a rota default.
- Prefixo maior = rede mais específica.
- Não confunda endereço de rede com primeiro host.
- Não confunda broadcast com último host.
- VLSM permite tamanhos diferentes.
- Sumarização reduz a quantidade de prefixos anunciados.
