# BGP — Border Gateway Protocol

## Conceito
BGP é um EGP baseado em atributos e política. Usa TCP/179.

- eBGP: entre AS diferentes.
- iBGP: dentro do mesmo AS.

## Atributos importantes
- Weight — Cisco local, não propagado.
- Local Preference — preferência de saída dentro do AS; maior é melhor.
- AS_PATH — caminho de AS; normalmente menor é preferido.
- Origin
- MED — sugestão de entrada; normalmente menor é melhor.
- eBGP sobre iBGP na seleção em condições equivalentes.
- Next-Hop
- Communities

## Seleção de rota — visão prática
A ordem exata depende do fabricante, mas em Cisco tradicional considere:
1. maior Weight
2. maior Local Preference
3. rota originada localmente
4. menor AS_PATH
5. menor Origin
6. menor MED
7. eBGP sobre iBGP
8. menor IGP metric até next-hop
9. critérios de desempate adicionais.

## Troubleshooting
```text
show ip bgp summary
show ip bgp
show ip bgp <prefix>
show ip route <prefix>
show ip bgp neighbors
```

Checklist:
1. TCP/179 alcança o vizinho?
2. Neighbor está Established?
3. ASN remoto está correto?
4. Address-family está ativa?
5. Prefixo foi anunciado?
6. Prefixo foi recebido?
7. Existe política route-map/prefix-list?
8. Next-hop é alcançável?
9. Existe filtro ou comunidade?
10. A rota escolhida é realmente a esperada?

## Route Reflector
Reduz necessidade de full-mesh iBGP. Clientes anunciam ao RR, que reflete rotas conforme as regras do BGP.

## Comunidades
Permitem transportar marcações de política, por exemplo para controlar preferência, exportação ou tratamento de rotas.
