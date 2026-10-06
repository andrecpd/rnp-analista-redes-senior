# LAB 08 — Monitoramento, Syslog e SNMP

## Objetivo

Praticar observabilidade.

## Métricas

- disponibilidade
- latência
- perda
- utilização
- CPU
- memória
- erros
- drops
- flaps
- sessões BGP/OSPF

## Cisco

~~~text
show processes cpu
show memory
show interfaces
show interfaces counters errors
show logging
~~~

## Syslog

~~~cisco
logging host <IP-SYSLOG>
logging trap informational
service timestamps log datetime msec
~~~

Linux:

~~~bash
journalctl -f
~~~

## SNMP

Entenda:
- OID
- MIB
- polling
- trap
- SNMPv2c
- SNMPv3

Não publique secrets no GitHub.

## Teste

Derrube uma interface e observe estado, log, impacto e recuperação.

## KPI

~~~text
Availability = uptime / período × 100
~~~

Compare MTTR, MTBF, disponibilidade, perda e latência.

## Desafio

Chamado: “O backbone está ficando saturado.”

Analise:
- utilização;
- tendência;
- horário de pico;
- erros;
- drops;
- capacidade;
- origem/destino;
- crescimento;
- impacto.
