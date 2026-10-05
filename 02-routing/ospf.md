# OSPF — Open Shortest Path First

## 1. Visão geral

OSPF é um **IGP de estado de enlace**, baseado em **SPF/Dijkstra**, usado para roteamento interno.

Para a prova, pense:

`Hello → vizinhança → adjacência → LSAs → LSDB → SPF → RIB/FIB`

Características:
- Link-state.
- Métrica **cost**.
- Hierarquia por áreas.
- **Area 0** = backbone.
- VLSM/CIDR.
- DR/BDR em redes multiacesso.
- OSPFv2 usa **IP protocol 89** — não TCP/UDP.

---

## 2. Estados de vizinhança

`Down → Init → 2-Way → ExStart → Exchange → Loading → Full`

| Estado | O que indica |
|---|---|
| Down | Nenhum Hello válido recebido |
| Init | Hello recebido, mas o próprio Router ID não aparece |
| 2-Way | Comunicação bidirecional confirmada |
| ExStart | Negociação para troca da LSDB |
| Exchange | Database Description sendo trocada |
| Loading | LSAs adicionais sendo solicitadas |
| Full | LSDB sincronizada |

### Pegadinha

**2-Way pode ser normal** em Ethernet broadcast: roteadores DROTHER normalmente formam Full com DR/BDR, não necessariamente entre todos.

---

## 3. DR e BDR

Em redes broadcast/multiacesso, o OSPF pode eleger:
- **DR** — Designated Router.
- **BDR** — Backup Designated Router.

Objetivo: reduzir adjacências e otimizar a distribuição de LSAs.

---

## 4. LSAs essenciais

| Tipo | Nome | Função |
|---|---|---|
| 1 | Router LSA | Links do roteador na área |
| 2 | Network LSA | Gerado pelo DR em rede multiacesso |
| 3 | Summary LSA | Prefixos entre áreas |
| 4 | ASBR Summary | Alcance até o ASBR |
| 5 | External | Rotas externas |
| 7 | NSSA External | Rotas externas em NSSA |

**ABR** conecta áreas.  
**ASBR** injeta rotas externas no OSPF.

---

## 5. Áreas

A **Area 0** é o backbone.

Exemplo:

`Area 1 → Area 0 → Area 2`

Boas práticas:
- manter o backbone estável;
- controlar tamanho das áreas;
- usar sumarização quando aplicável;
- evitar desenhos que prejudiquem a conectividade lógica com Area 0.

---

## 6. Métrica

O OSPF prefere o caminho de **menor custo total**.

Exemplo:

`R1--10--R2--10--R4` = cost 20

`R1--------50--------R4` = cost 50

O caminho de cost 20 é preferido.

O cálculo exato do cost depende da referência de banda e da implementação/configuração.

---

## 7. Troubleshooting — método sênior

Se uma rota não aparece, siga:

`Interface → IP → Neighbor → LSDB → SPF → RIB/FIB`

Perguntas:
1. Interface está UP/UP?
2. Existe conectividade IP?
3. Vizinho aparece?
4. Adjacência chegou a Full?
5. LSA esperada existe?
6. SPF encontrou caminho?
7. Existe outra rota melhor?
8. Prefixo foi instalado na RIB/FIB?

---

## 8. ExStart — pegadinha clássica

Vizinho preso em **ExStart**:

Primeira hipótese importante: **MTU incompatível**.

Também verifique:
- área;
- timers;
- autenticação;
- network type;
- Router ID;
- conectividade;
- ACL/firewall.

Não conclua apenas pelo estado: confirme com evidências.

---

## 9. Adjacência não forma — checklist

```
[ ] Interface UP/UP
[ ] IP/máscara corretos
[ ] Conectividade entre vizinhos
[ ] Área igual
[ ] Hello/Dead timers compatíveis
[ ] Autenticação compatível
[ ] Network type compatível
[ ] MTU compatível
[ ] Router ID válido
[ ] ACL/firewall não bloqueando OSPF
```

---

## 10. Comandos Cisco

```
show ip ospf neighbor
show ip ospf interface
show ip ospf interface brief
show ip ospf
show ip ospf database
show ip route ospf
show ip route <prefixo>
```

Interpretação:
- **neighbor** → estado da adjacência;
- **interface** → timers, cost, network type;
- **database** → LSAs/LSDB;
- **route ospf** → rotas OSPF instaladas.

---

## 11. OSPF x BGP

| OSPF | BGP |
|---|---|
| IGP | EGP/path-vector |
| Link-state | Path-vector |
| SPF/Dijkstra | Atributos/políticas |
| Cost | Local Pref, AS_PATH, MED etc. |
| IP protocol 89 | TCP/179 |

---

## 12. Pegadinhas da prova

- OSPF **não usa TCP/179**.
- OSPFv2 usa **IP protocol 89**.
- **2-Way pode ser normal** com DR/BDR.
- Full significa **LSDB sincronizada**, não que toda rota desejada esteja instalada.
- ExStart → investigue **MTU**.
- Menor **cost total** é preferido.
- Neighbor Full não garante que um prefixo específico esteja sendo anunciado.

---

## 13. Resposta de nível sênior

Pergunta: “OSPF está Full, mas 10.10.20.0/24 não aparece. O que faria?”

Resposta:

`Confirmar escopo → verificar anúncio → consultar LSDB → validar LSAs → analisar SPF/cost → verificar outra rota preferível → validar RIB/FIB → corrigir → testar → documentar`

### Regra de ouro

> **Não confunda vizinhança OSPF com disponibilidade da rota.**

---

## 14. Resumo de 30 segundos

```
OSPF = Link-State + SPF/Dijkstra
IP protocol = 89
Backbone = Area 0
Métrica = Cost
Broadcast = DR/BDR
Full = LSDB sincronizada
ExStart = verificar MTU
2-Way = pode ser normal
LSA = informação de topologia/prefixos
Troubleshooting = Neighbor → LSDB → SPF → RIB/FIB
```
