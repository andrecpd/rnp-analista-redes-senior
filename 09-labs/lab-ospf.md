# Lab OSPF

## Topologia
R1 — R2 — R3
- Área 0.
- Loopbacks como Router-ID.
- R1 e R3 anunciam redes de teste.

## Objetivos
1. Formar adjacência.
2. Validar LSDB.
3. Alterar custo.
4. Observar mudança de caminho.
5. Simular falha de vizinho.

## Validação
```text
show ip ospf neighbor
show ip ospf database
show ip route ospf
```

## Perguntas
- Por que a adjacência ficou em ExStart?
- Qual LSA aparece para a rede multiaccess?
- O que acontece quando a interface do caminho preferido cai?
