# Trilha de laboratórios BGP — RNP Analista de Redes Sênior

Laboratórios gratuitos, progressivos e orientados a troubleshooting, utilizando FRRouting (FRR) em GNS3 ou EVE-NG.

## Sequência planejada

| Lab | Tema | Conteúdo principal | Estado |
|---|---|---|---|
| [BGP-01](01-ebgp-basico/README.md) | eBGP básico | Sessão, anúncios IPv4, AS_PATH, diagnóstico inicial | Disponível |
| BGP-02 | eBGP com três ASNs | Propagação, AS_PATH e prevenção de loops | Planejado |
| BGP-03 | Seleção de melhor caminho | LOCAL_PREF, AS_PATH, ORIGIN, MED e NEXT_HOP | Planejado |
| BGP-04 | Políticas de roteamento | Prefix-list, route-map e filtros inbound/outbound | Planejado |
| BGP-05 | iBGP e route reflector | Full mesh, split-horizon, RR e next-hop | Planejado |
| BGP-06 | ISP com dois upstreams | Redundância, preferência de entrada/saída e failover | Planejado |
| BGP-07 | BGP + OSPF | Underlay, next-hop reachability e troubleshooting | Planejado |
| BGP-08 | MP-BGP IPv6 | Address-family IPv6 e validação | Planejado |
| BGP-09 | Segurança de roteamento | Prefix filtering, max-prefix, RPKI/ROV (conceitos e prática compatível) | Planejado |
| BGP-10 | Simulado RNP | Cenário integrado, incidentes e tempo limitado | Planejado |

## Método obrigatório

Para cada lab: desenhar → configurar → validar → introduzir uma falha → coletar evidências → formular hipótese → corrigir → validar novamente → documentar causa raiz.

## Ambiente e legalidade

- FRR é software livre e tem documentação pública: https://docs.frrouting.org/en/stable-10.2/bgp.html
- GNS3: https://docs.gns3.com/
- EVE-NG: https://www.eve-ng.net/
- Imagens comerciais de roteadores devem ser obtidas legalmente; não distribua imagens proprietárias no repositório.
- Os arquivos deste curso são exemplos didáticos e devem ser testados em laboratório isolado, nunca aplicados diretamente em produção.
