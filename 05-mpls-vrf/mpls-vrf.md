# MPLS e VRF — Resumo RNP Sênior

## 1. Visão mental

`CE → PE → P → P → PE → CE`

- **CE (Customer Edge):** equipamento do cliente.
- **PE (Provider Edge):** mantém VRFs e participa do MP-BGP.
- **P (Provider):** transporta labels no core; normalmente não precisa conhecer as rotas dos clientes.
- **MPLS:** encaminha usando labels associados a FECs.
- **FEC:** classe de equivalência de encaminhamento.
- **LSP:** caminho lógico de labels.
- **LER:** entrada/saída do domínio MPLS.
- **LSR:** encaminha/swap de labels.

### O que lembrar na prova

> O core MPLS pode transportar tráfego de vários clientes sem colocar todas as rotas dos clientes na tabela global dos roteadores P.

## 2. Operações de label

Conceitos:
- **Push:** adiciona label.
- **Swap:** troca label por outro.
- **Pop:** remove label.
- **PHP (Penultimate Hop Popping):** o penúltimo roteador pode remover o label externo antes do PE.

Fluxo simplificado:

`IP → Push → Swap → Swap → Pop/PHP → IP`

## 3. LDP

O **LDP** distribui labels para FECs e normalmente trabalha junto com um IGP para alcançar os next-hops.

Pontos de atenção:
- vizinhança LDP;
- Router ID;
- transporte/TCP;
- reachability dos loopbacks;
- labels presentes;
- LSP completo.

Comandos Cisco:
```text
show mpls ldp neighbor
show mpls ldp discovery
show mpls forwarding-table
show mpls interfaces
show mpls ldp bindings
show ip route
```

## 4. VRF

**VRF = Virtual Routing and Forwarding.**

Permite tabelas de roteamento independentes no mesmo equipamento.

Exemplo:
```text
show vrf
show ip route vrf CLIENTE-A
show ip route vrf CLIENTE-B
ping vrf CLIENTE-A 10.10.10.1
```

### VRF ≠ VLAN

- VLAN separa principalmente **domínios L2**.
- VRF separa **tabelas L3**.
- É possível combinar VLAN + VRF.

## 5. MPLS L3VPN

Arquitetura:

`CE → PE → MPLS Core → PE → CE`

No PE:
1. interface do cliente entra em uma VRF;
2. rota do cliente é aprendida;
3. rota recebe **RD** para formar uma rota VPN distinguível;
4. **RT** determina import/export entre VRFs;
5. MP-BGP transporta VPNv4/VPNv6;
6. labels permitem o transporte pelo core.

### RD x RT — pegadinha clássica

| Item | Função |
|---|---|
| **RD** | torna rotas VPN distinguíveis |
| **RT** | controla import/export de rotas |
| **MP-BGP** | distribui rotas VPN |
| **LDP/MPLS** | fornece transporte pelo core |

## 6. Troubleshooting L3VPN

### Caso: CE-A não alcança CE-B

Ordem recomendada:

1. Interface CE-PE está UP?
2. Rota existe na VRF de origem?
3. Rota foi aprendida pelo PE?
4. MP-BGP está Established?
5. RT de export/import está correto?
6. Rota aparece na VRF remota?
7. Next-hop é alcançável?
8. LDP/LSP está funcional?
9. Labels existem no forwarding table?
10. ACL/QoS/firewall está bloqueando?
11. Teste origem-destino usando a VRF.

### Regra sênior

> Se a rota existe na VRF, não pare no controle. Verifique também o plano de encaminhamento: next-hop, labels/LSP e interface de saída.

## 7. Pegadinhas de prova

- MPLS não é simplesmente "criptografia".
- LDP distribui labels; não substitui o IGP.
- VRF separa tabelas de roteamento.
- PE conhece VRFs; P normalmente transporta labels.
- RD não é filtro de importação.
- RT controla import/export.
- MP-BGP transporta informação VPN.
- Rota na VRF não garante forwarding funcional.
- LDP neighbor não significa automaticamente L3VPN operacional.

## 8. Resumo de 30 segundos

`VRF = tabela separada`

`MPLS = labels`

`LDP = distribuição de labels`

`MP-BGP = rotas VPN`

`RD = distingue`

`RT = controla import/export`

`PE = inteligência VPN`

`P = transporte MPLS`

**Golden rule:** CE-PE → VRF → MP-BGP/RT → next-hop → LDP/LSP → labels → forwarding.
