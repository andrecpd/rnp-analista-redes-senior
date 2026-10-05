# Monitoramento e Observabilidade — RNP

## 1. Objetivo

Monitoramento responde **"o que está acontecendo?"**.

Observabilidade ajuda a responder **"por que está acontecendo?"**.

Em redes, combine:
- métricas;
- logs;
- eventos;
- estados de protocolos;
- traces/capturas quando disponíveis.

## 2. SNMP

Arquitetura:
`NMS/Manager ↔ Agent ↔ equipamento`

| Item | Uso |
|---|---|
| UDP/161 | consultas SNMP |
| UDP/162 | traps/informs |
| SNMPv2c | community string; sem segurança moderna suficiente |
| SNMPv3 | autenticação e privacidade |

Comandos/conceitos que o analista deve conhecer:
- OID;
- MIB;
- polling;
- trap;
- inform;
- interface counters;
- uptime.

**Boa prática:** preferir SNMPv3 quando suportado.

## 3. O que monitorar em rede

### Interface
- utilização;
- input/output errors;
- CRC;
- discards;
- drops;
- flaps;
- velocidade/duplex;
- óptica, quando disponível.

### Equipamento
- CPU;
- memória;
- temperatura;
- fontes/fans;
- uptime;
- processos;
- capacidade de TCAM/forwarding, quando aplicável.

### Routing
- BGP Established/Idle;
- OSPF neighbor state;
- número de rotas;
- mudanças de adjacência;
- flaps;
- reachability.

### Serviço
- latência;
- perda;
- jitter;
- DNS;
- TCP connect;
- disponibilidade da aplicação.

## 4. Syslog

Centralizar logs permite:
- correlação temporal;
- investigação de incidentes;
- identificação de flaps;
- auditoria;
- análise de causa raiz.

Procure correlação:

`14:03 link down → 14:03 OSPF down → 14:03 BGP down → 14:04 rotas reconvergiram`

Isso é mais útil que olhar eventos isolados.

## 5. Alertas de qualidade

Um bom alerta tem:
- severidade;
- ativo afetado;
- horário;
- métrica;
- limiar;
- duração;
- impacto;
- runbook/link para ação.

### Ruído

Alerta que dispara frequentemente sem ação útil = **noise**.

Objetivo do NOC não é ter mais alertas; é ter alertas **acionáveis**.

## 6. KPIs operacionais

- **Disponibilidade:** percentual de tempo operacional.
- **MTTR:** tempo médio para restaurar/reparar.
- **MTBF:** tempo médio entre falhas.
- **Latência:** tempo de ida/resposta conforme método.
- **Jitter:** variação do atraso.
- **Packet loss:** perda de pacotes.
- **Utilização:** consumo de capacidade.
- **Flap rate:** frequência de mudanças de estado.

Aproximação:

`Disponibilidade = MTBF / (MTBF + MTTR)`

Exemplo: MTBF=99h e MTTR=1h → aproximadamente 99%.

## 7. Baseline

Antes de declarar anomalia, compare com o comportamento normal.

Exemplo:
- interface a 70% sempre → pode ser normal;
- interface normalmente a 20% e hoje 90% → forte indício de mudança.

Baseline deve considerar:
- horário;
- dia da semana;
- sazonalidade;
- janela de backup;
- crescimento.

## 8. Capacidade

Não olhe somente para percentual atual.

Avalie:
`capacidade atual → crescimento → pico → headroom → redundância → prazo de expansão`

Pergunta sênior:
> "Quanto tempo temos antes de atingir o limite operacional?"

## 9. Troubleshooting orientado por observabilidade

`Alerta → escopo → correlação → evidência → hipótese → teste → correção → validação`

## 10. Pegadinhas

- CPU alta não prova que CPU é a causa.
- Perda SNMP não significa necessariamente perda de tráfego.
- Um alerta isolado não substitui correlação.
- Alta utilização não significa automaticamente saturação.
- Interface sem erro físico ainda pode estar congestionada.
- Disponibilidade não é sinônimo de desempenho.

## Resumo de 30 segundos

**Métrica mostra comportamento. Log mostra eventos. Estado de protocolo mostra controle. Captura mostra pacotes. Correlação transforma sinais em diagnóstico.**
