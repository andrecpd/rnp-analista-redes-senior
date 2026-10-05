# Troubleshooting de Redes — Método Sênior

## 1. Regra principal

**Não comece pela configuração. Comece pelo sintoma e pela evidência.**

Fluxo:

`Sintoma → Escopo → Evidência → Hipóteses → Teste → Causa raiz → Correção → Validação → Prevenção`

## 2. Passo 1 — Defina o sintoma

Pergunte:
- quem é afetado?
- desde quando?
- é total ou intermitente?
- qual serviço?
- qual origem/destino?
- houve mudança antes da falha?

## 3. Passo 2 — Determine o escopo

Compare:
- um host vs vários;
- uma VLAN vs várias;
- um POP vs vários;
- um link vs vários;
- um destino vs vários.

**Escopo reduz drasticamente o espaço de hipóteses.**

## 4. Passo 3 — Colete evidências

Exemplos:
- interface counters;
- MAC/ARP/NDP;
- tabela de rotas;
- OSPF/BGP neighbors;
- logs;
- SNMP;
- traceroute/mtr;
- captura de pacotes;
- estado da aplicação.

## 5. Passo 4 — Formule hipóteses

Não tenha apenas uma.

Exemplo: perda de pacotes:
1. físico;
2. congestionamento;
3. policer/QoS;
4. MTU;
5. CPU;
6. assimetria;
7. firewall.

Classifique por **probabilidade × impacto × facilidade de teste**.

## 6. Passo 5 — Teste menos invasivo

Prefira:
`show/read-only → teste controlado → captura → mudança`

Evite:
- reboot sem evidência;
- shutdown de interfaces sem plano;
- limpeza indiscriminada de sessões;
- mudanças simultâneas.

## 7. Camadas

`Físico → L2 → L3 → Transporte → Aplicação`

Mas não trate OSI como uma sequência rígida. Use a camada que melhor explica a evidência.

## 8. Exemplo — usuário sem acesso à aplicação

```text
Link/interface
   ↓
VLAN
   ↓
IP/DHCP
   ↓
ARP/NDP
   ↓
Gateway
   ↓
Rota
   ↓
ACL/Firewall
   ↓
TCP/UDP
   ↓
DNS
   ↓
Aplicação
```

## 9. Perda de pacotes

Diferencie:
- **erro físico/CRC:** forte indício de problema L1/L2;
- **drop por congestionamento:** fila/recursos;
- **policer:** descarte intencional por política;
- **MTU:** falhas seletivas;
- **ICMP rate-limit:** pode afetar teste sem afetar forwarding.

## 10. Alta latência

Compare origem → trânsito → destino.

Não conclua que um hop com latência alta é causa se os hops seguintes e o destino estão normais.

Use:
- ping;
- traceroute;
- mtr;
- counters;
- telemetria;
- captura quando necessário.

## 11. TCP

Separe:
- DNS resolve?
- IP alcançável?
- SYN sai?
- SYN/ACK volta?
- ACK completa handshake?
- TLS funciona?
- aplicação responde?

Uma captura pode responder mais que vários testes de ping.

## 12. Validação

Depois da correção:
1. repetir o teste original;
2. testar caminho alternativo;
3. verificar métricas;
4. observar estabilidade;
5. confirmar com usuário/aplicação;
6. registrar resultado.

## 13. Pós-incidente

Documente:
- impacto;
- timeline;
- causa raiz;
- causa contribuinte;
- ação corretiva;
- rollback, se ocorreu;
- evidências;
- prevenção;
- owner;
- próximos passos.

## 14. Pegadinhas RNP

- Ping funcionando não prova aplicação funcionando.
- BGP Established não prova que o prefixo está na RIB/FIB.
- OSPF Full não prova que todo prefixo está instalado.
- Traceroute não é prova absoluta de falha em um hop.
- Alta CPU não prova causa raiz.
- Uma mudança pode ser causa, mas precisa ser correlacionada no tempo e por evidência.

## Resposta sênior em uma frase

> "Vou delimitar o impacto, coletar evidências, testar hipóteses de menor risco, identificar a causa raiz, corrigir de forma controlada e validar o serviço antes de encerrar."
