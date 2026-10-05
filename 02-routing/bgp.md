# BGP — Border Gateway Protocol

## 1. Visão geral

BGP é um protocolo **path-vector**, orientado a **políticas e atributos**.

Para a prova:

`TCP/179 → sessão → troca de rotas → políticas → best path → RIB/FIB`

Características:
- **TCP/179**.
- **eBGP** entre AS diferentes.
- **iBGP** dentro do mesmo AS.
- Alta escalabilidade.
- Seleção baseada em atributos.
- Controle de política de roteamento.

---

## 2. eBGP x iBGP

| | eBGP | iBGP |
|---|---|---|
| AS | Diferentes | Mesmo AS |
| Uso | Entre domínios | Distribuição interna |
| TCP | 179 | 179 |
| AS_PATH | Recebido entre AS | Não é incrementado entre vizinhos iBGP |

Em redes grandes, são comuns **Route Reflectors**.

---

## 3. Estados BGP

`Idle → Connect → Active → OpenSent → OpenConfirm → Established`

### Pegadinha

**Established significa sessão estabelecida, não que as rotas estejam corretas.**

Uma sessão pode estar Established e existir:
- zero prefixos;
- filtro de prefixos;
- route-map bloqueando;
- address-family inativa;
- next-hop inalcançável;
- rota perdendo no best path.

---

## 4. Atributos essenciais

### Weight
- Típico de Cisco.
- Local ao roteador.
- Não propagado.
- **Maior é melhor.**

### Local Preference
- Política de saída dentro do AS.
- **Maior é melhor.**

### AS_PATH
- Lista de AS atravessados.
- Ajuda na prevenção de loops.
- Em condições equivalentes, **menor tende a ser melhor**.

### Origin
Visão simplificada:

`IGP < EGP < Incomplete`

### MED
- Sugere ao AS vizinho uma preferência de entrada.
- Normalmente **menor é melhor**.

### Communities
Marcam rotas para aplicação de políticas.

---

## 5. Best Path — visão Cisco tradicional

A ordem exata pode variar por fabricante/versão, mas para a prova Cisco memorize:

```
1. Maior Weight
2. Maior Local Preference
3. Originação local
4. Menor AS_PATH
5. Melhor Origin
6. Menor MED
7. eBGP sobre iBGP
8. Menor IGP metric até o next-hop
9. Desempates adicionais
```

### Exemplo

Path A:
- Local Preference 200
- AS_PATH 65010 65020

Path B:
- Local Preference 100
- AS_PATH 65030

**A vence pelo Local Preference**, mesmo com AS_PATH maior.

---

## 6. Next-Hop

Uma rota BGP pode ter next-hop que precisa ser alcançado por IGP/rota estática.

Verifique:

`show ip route <next-hop>`

Raciocínio:

`Prefixo recebido → next-hop alcançável? → best path → RIB/FIB`

---

## 7. Troubleshooting BGP

### Etapa 1 — sessão

```
show ip bgp summary
```

Verifique:
- vizinho;
- estado;
- ASN;
- prefixos recebidos/enviados.

### Etapa 2 — prefixo

```
show ip bgp <prefixo>
show ip route <prefixo>
```

Analise:
- caminhos recebidos;
- atributos;
- best path;
- next-hop;
- instalação na RIB.

### Etapa 3 — política

Investigue:
- prefix-list;
- route-map/policy;
- communities;
- filtros AS_PATH;
- address-family;
- inbound/outbound policy.

---

## 8. BGP Established, mas prefixo não chega

Checklist:

```
[ ] TCP/179 OK
[ ] Neighbor Established
[ ] ASN correto
[ ] Address-family ativa
[ ] Prefixo realmente anunciado
[ ] Prefix-list permite
[ ] Route-map/policy permite
[ ] Community não provoca filtro
[ ] Prefixo aparece no BGP do vizinho
[ ] Next-hop alcançável
[ ] Não foi rejeitado por política
[ ] Não perdeu para outro caminho
[ ] Foi instalado na RIB/FIB
```

---

## 9. Comandos Cisco

```
show ip bgp summary
show ip bgp
show ip bgp <prefixo>
show ip route <prefixo>
show ip bgp neighbors
show ip bgp neighbors <IP> advertised-routes
show ip bgp neighbors <IP> received-routes
show ip route <next-hop>
show running-config | section router bgp
```

O suporte/impacto de comandos de rotas recebidas pode variar por plataforma e configuração.

---

## 10. Route Reflector

O iBGP tradicional exige full-mesh lógico.

Com Route Reflector:

`R1 ─┐`
`R2 ─┼─ RR`
`R3 ─┘`

Benefícios:
- menos sessões;
- melhor escalabilidade;
- operação mais simples.

RR não substitui:
- IGP adequado;
- políticas;
- controle de loops;
- planejamento de arquitetura.

---

## 11. Communities

Fluxo conceitual:

`Prefixo + Community → Policy → Ação`

Usos:
- controle de anúncios;
- preferência;
- exportação/importação;
- filtragem;
- tratamento especial.

---

## 12. BGP x OSPF

| BGP | OSPF |
|---|---|
| Path-vector | Link-state |
| Política/atributos | SPF/cost |
| TCP/179 | IP protocol 89 |
| Inter-AS e políticas | Roteamento interno |
| AS_PATH/Local Pref/MED | LSAs/cost/áreas |

---

## 13. Cenários clássicos

### BGP Established, zero prefixos
Investigue:

`Address-family → anúncio → filtros → policy → communities`

### Prefixo recebido, mas não instalado
Investigue:

`Best path → next-hop → policy → outra rota → RIB/FIB`

### Caminho inesperado
Compare:

`Weight → Local Preference → origem local → AS_PATH → Origin → MED → eBGP/iBGP → IGP metric`

---

## 14. Pegadinhas da prova

- BGP usa **TCP/179**.
- Established ≠ rotas corretas.
- **Maior Local Preference** é melhor.
- **Maior Weight** é melhor no Cisco e é local.
- **Menor AS_PATH** tende a ser melhor depois dos atributos anteriores.
- **Menor MED** tende a ser melhor.
- Next-hop precisa ser alcançável.
- Prefix-list/route-map pode bloquear uma rota sem derrubar a sessão.
- iBGP ≠ eBGP.
- Route Reflector reduz a necessidade de full-mesh iBGP.

---

## 15. Resposta de nível sênior

Pergunta: “BGP está Established, mas 10.20.30.0/24 não aparece. Como investigaria?”

`Sessão → address-family → anúncio → filtros → policy → BGP table → next-hop → best path → RIB/FIB → validação → causa raiz`

### Regra de ouro

> **BGP Established prova a sessão TCP/BGP, não prova que a política de roteamento está correta.**

---

## 16. Resumo de 30 segundos

```
BGP = Path Vector + Política
TCP = 179
eBGP = AS diferente
iBGP = mesmo AS
Weight = maior, local Cisco
Local Preference = maior
AS_PATH = menor, em condições equivalentes
MED = menor, em condições equivalentes
Next-hop = alcançável
RR = escala iBGP
Established ≠ rotas corretas
Troubleshooting = sessão → AF → anúncio → filtros → atributos → next-hop → RIB/FIB
```
