# LAB-BGP-01 — Gabarito comentado

> Tente concluir os testes antes de abrir esta página.

## Resultado esperado

- R1 (AS 65001) estabelece eBGP com R2 (AS 65002) via TCP/179.
- R1 anuncia `192.0.2.0/24`; R2 anuncia `198.51.100.0/24`.
- R1 aprende o prefixo de R2 com AS_PATH `65002`; R2 aprende o prefixo de R1 com AS_PATH `65001`.
- Ambos os prefixos são elegíveis para instalação na tabela IP; ping entre loopbacks funciona se o sistema e as interfaces estiverem configurados corretamente.

## Falha 1 — ASN remoto incorreto

**Sintoma:** sessão não estabelece; os logs ou mensagens de notificação indicam incompatibilidade de AS, ou a sessão reinicia.

**Causa:** o `remote-as` configurado localmente não corresponde ao ASN real do vizinho.

**Correção:** em R1, configurar `neighbor 10.0.12.2 remote-as 65002`. Confirmar em `show bgp summary`.

## Falha 2 — Prefixo não anunciado

**Sintoma:** peer Established, mas o prefixo esperado não aparece no vizinho.

**Causas prováveis:** comando `network` ausente, família IPv4 não ativada, prefixo exato inexistente na RIB local, ou política de saída bloqueando o anúncio.

**Correção:** validar `show running-config`, `show ip route` e `show bgp ipv4 unicast`. Repor `network 192.0.2.0/24` em R1 e garantir que a rota conectada existe.

## Falha 3 — Prefixo não existe localmente

O BGP não anuncia a rota via `network` se o prefixo correspondente não estiver na tabela de roteamento local. Repor o endereço `192.0.2.1/24` na interface loopback de R1 e confirmar a rota conectada.

## Falha 4 — TCP/179 bloqueado

**Sintoma:** a sessão não chega a Established, mesmo com endereçamento e ASN corretos.

**Causa:** filtro/firewall bloqueando o TCP usado pelo BGP, ou ausência de conectividade até o peer.

**Correção:** remover a regra de laboratório e confirmar conectividade IP e TCP/179.

## Perguntas de prova — respostas

1. **eBGP** conecta sistemas autônomos diferentes; **iBGP** distribui rotas BGP dentro do mesmo AS.
2. TCP, porta 179 por padrão.
3. Established significa que a sessão foi estabelecida e os peers podem trocar atualizações BGP.
4. O anúncio pode estar ausente por `network`/RIB local, família de endereços não ativada, filtros, políticas ou falta de rota elegível.
5. AS_PATH registra os AS atravessados; um AS normalmente rejeita rotas que já contêm seu próprio ASN, prevenindo loops inter-AS.
6. A seleção BGP e a instalação na RIB dependem de validade, alcance do NEXT_HOP, política e preferência por rotas concorrentes. A FIB é a estrutura usada para encaminhar pacotes.
7. Coletar estado do peer, logs, endereçamento/rotas, tabela BGP, configuração e captura TCP/179 antes de mudar parâmetros.

## Critério de aprovação

Considere concluído quando:
- [ ] Ambos os peers estão Established.
- [ ] Cada roteador aprende o prefixo remoto.
- [ ] AS_PATH está correto.
- [ ] Ping entre loopbacks funciona.
- [ ] Duas falhas foram diagnosticadas e corrigidas com evidência.
- [ ] O candidato explica cada etapa sem decorar apenas comandos.
