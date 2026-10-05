# OSPF — Open Shortest Path First

## Características
- IGP baseado em estado de enlace.
- Usa algoritmo SPF/Dijkstra.
- Métrica principal: cost.
- Usa áreas para escalar.
- Backbone é a Area 0.

## Adjacência
Estados comuns:
Down → Init → 2-Way → ExStart → Exchange → Loading → Full.

Em redes broadcast, DR/BDR reduzem adjacências necessárias.

## LSA
Conheça principalmente:
- Type 1: Router LSA
- Type 2: Network LSA
- Type 3: Summary inter-area
- Type 4: ASBR summary
- Type 5: External
- Type 7: NSSA external

## Troubleshooting
Verifique:
```text
show ip ospf neighbor
show ip ospf interface
show ip ospf database
show ip route ospf
```

Causas típicas de vizinhança não formar:
- área diferente
- timers incompatíveis
- autenticação
- MTU
- network type
- router-id duplicado
- conectividade IP

## Métrica
Cost é associado à interface. O caminho com menor custo total é preferido.

## Pergunta clássica
Se o vizinho está em 2-Way em uma rede broadcast, isso pode ser normal entre roteadores que não precisam formar Full diretamente por causa de DR/BDR.
