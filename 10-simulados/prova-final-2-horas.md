# Simulado Final — 2 Horas — RNP Analista de Redes Sênior

## Regras

- **40 questões**
- **120 minutos**
- **Sem consulta**
- Uma alternativa por questão
- Reserve **10 minutos finais** para revisão

## Distribuição

| Questões | Tema | Tempo-alvo |
|---|---|---:|
| 1–8 | IPv4 / Subnetting / Fundamentos | 20 min |
| 9–15 | Switching / VLAN / STP | 18 min |
| 16–22 | OSPF | 18 min |
| 23–30 | BGP | 24 min |
| 31–34 | IPv6 | 10 min |
| 35–37 | MPLS / VRF | 8 min |
| 38–39 | Linux / Monitoramento | 5 min |
| 40 | Troubleshooting | 2 min |
| Revisão | Todas | 15 min |

> Os tempos são referência. Não deixe uma questão difícil consumir o tempo das demais.

## Estratégia de prova

### 1ª passagem — certeza

Responda rapidamente:
- conceitos diretos;
- portas/protocolos;
- definições;
- comandos;
- cálculos simples.

### 2ª passagem — raciocínio

Volte para:
- subnetting;
- seleção de rotas;
- BGP best-path;
- OSPF;
- cenários de troubleshooting.

### 3ª passagem — revisão

Para cada dúvida:
1. elimine alternativas tecnicamente impossíveis;
2. procure palavras absolutas como **"sempre"**, **"nunca"**, **"somente"**;
3. confirme se a alternativa responde exatamente ao enunciado;
4. não troque uma resposta correta sem evidência.

## Checklist técnico de última hora

### IPv4
- /24 = 254 hosts
- /25 = 126
- /26 = 62
- /27 = 30
- /28 = 14
- /29 = 6
- /30 = 2

### OSPF
`Hello → Neighbor → LSDB → SPF → RIB/FIB`

Lembre:
- protocol 89;
- Area 0;
- DR/BDR;
- 2-Way pode ser normal em broadcast;
- ExStart → verificar MTU primeiro;
- Full não garante todos os prefixos.

### BGP
`TCP/179 → Established → Prefixes → Policy → Best Path → RIB/FIB`

Lembre:
- Weight;
- Local Preference;
- AS_PATH;
- Origin;
- MED;
- eBGP/iBGP;
- next-hop;
- prefix-list/route-map.

### Switching
`Interface → VLAN → Trunk → STP → MAC → SVI`

### IPv6
- 128 bits;
- NDP/ICMPv6;
- FE80::/10 link-local;
- FF00::/8 multicast;
- RA/SLAAC;
- PMTUD.

### MPLS/VRF
`VRF → RD/RT → MP-BGP → LDP/LSP → Labels`

### Linux
- `ip addr`
- `ip route`
- `ip route get`
- `ip neigh`
- `ss`
- `dig`
- `tcpdump`
- `curl`

## Regra de ouro

**Sintoma → Evidência → Hipótese → Teste → Causa raiz → Correção → Validação.**

A questão de troubleshooting geralmente procura a **primeira ação correta**, não a mudança mais agressiva.
