# Lab BGP

## Topologia
AS 65001 — AS 65002 — AS 65003

## Objetivos
- Formar eBGP.
- Anunciar loopback.
- Verificar AS_PATH.
- Aplicar Local Preference em cenário iBGP.
- Testar prefix-list.
- Testar route-map.
- Analisar next-hop.

## Validação
```text
show ip bgp summary
show ip bgp
show ip bgp <prefixo>
show ip route <prefixo>
```

## Exercício
Faça o mesmo prefixo chegar por dois caminhos. Determine qual caminho será escolhido e explique com base nos atributos BGP.
