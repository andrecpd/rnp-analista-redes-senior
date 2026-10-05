# Monitoramento e Observabilidade

## SNMP
- Manager consulta agentes.
- UDP/161: operações de consulta.
- UDP/162: traps/informs.
- SNMPv3 deve ser preferido quando disponível por segurança.

## Syslog
Centraliza eventos e facilita correlação de incidentes.

## Métricas
- disponibilidade
- utilização de interface
- erros/discards
- CPU/memória
- latência
- perda
- jitter
- temperatura
- BGP/OSPF neighbor state

## NOC
Um bom alerta deve ser:
- acionável
- contextualizado
- com severidade
- com limiar adequado
- sem excesso de ruído

## Indicadores
MTTR = tempo médio para recuperação.
MTBF = tempo médio entre falhas.

Disponibilidade aproximada:
Disponibilidade = MTBF / (MTBF + MTTR)
