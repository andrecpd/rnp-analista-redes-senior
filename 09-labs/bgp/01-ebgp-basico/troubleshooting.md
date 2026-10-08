# LAB-BGP-01 — Folha de troubleshooting

Preencha antes de consultar o gabarito.

| Sintoma | Evidência coletada | Hipótese | Teste executado | Causa raiz | Correção |
|---|---|---|---|---|---|
| Peer não fica Established | | | | | |
| Sessão Established, prefixo ausente | | | | | |
| Prefixo aparece em BGP, mas não na tabela IP | | | | | |
| Prefixo recebido, ping falha | | | | | |

## Sequência de diagnóstico recomendada

1. **Camada 3:** `ip -br addr`, `ip route`, ping entre endereços do enlace.
2. **TCP:** confirme alcance TCP/179, firewall e estado de conexão.
3. **Sessão BGP:** `show bgp summary`, `show bgp neighbors`, logs do FRR.
4. **Anúncio:** `show running-config`, `show bgp ipv4 unicast`; confirme se o prefixo existe localmente.
5. **Política:** procure filtros, route-maps ou prefix-lists (não configurados no cenário inicial).
6. **Instalação:** `show ip route` e `ip route get <destino>`.
7. **Plano de dados:** ping entre loopbacks e traceroute; valide retorno e endereçamento.

## Comandos úteis

```bash
ip -br addr
ip route
ip route get 198.51.100.1
ping -c 3 10.0.12.2
sudo ss -tnp | grep ':179'
sudo journalctl -u frr --since '10 minutes ago'
```

```text
show bgp summary
show bgp neighbors
show bgp ipv4 unicast
show ip route
show running-config
```

## Relatório de incidente

- Sintoma e impacto:
- Horário e duração:
- Evidências:
- Hipóteses consideradas:
- Testes e resultados:
- Causa raiz confirmada:
- Correção aplicada:
- Validação pós-correção:
- Prevenção:
