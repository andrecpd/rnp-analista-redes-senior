# LAB 10 — SIMULADO PRÁTICO RNP — 2 HORAS

## Tempo

120 minutos, sem consulta.

| Etapa | Tempo |
|---|---:|
| Topologia | 10 min |
| L2 | 15 min |
| OSPF | 15 min |
| BGP | 20 min |
| IPv6 | 10 min |
| MPLS/VRF | 20 min |
| Linux | 15 min |
| Troubleshooting | 15 min |

## Regras

- Sem Google.
- Sem ChatGPT.
- Sem documentação.
- Use apenas comandos conhecidos.
- Registre evidências.

## Parte 1 — Switching

Crie VLAN 10, VLAN 20, trunk e RSTP.

## Parte 2 — OSPF

Configure três roteadores com Area 0, Router-ID, loopbacks e redundância.

Valide:

~~~text
show ip ospf neighbor
show ip route ospf
~~~

## Parte 3 — BGP

Forme eBGP e anuncie um /24.

~~~text
show ip bgp summary
show ip bgp
~~~

## Parte 4 — IPv6

Configure IPv6 e valide rota e ping.

## Parte 5 — MPLS/VRF

Crie VRF e transporte rota de cliente pelo core.

~~~text
show ip route vrf <VRF>
show mpls ldp neighbor
~~~

## Parte 6 — Linux

Descubra por que Client → Server não acessa TCP/8080.

~~~bash
ip route
ip neigh
ss -lntup
curl -v
tcpdump
~~~

## Parte 7 — Troubleshooting

O avaliador introduz três falhas sem informar quais.

Apresente:
1. sintoma;
2. evidência;
3. hipótese;
4. teste;
5. causa;
6. correção;
7. validação.

## Critério

### Excelente
Configura sem consulta, usa comandos corretos, raciocina por camadas, encontra causa raiz e documenta evidências.

### Bom
Configura corretamente e consegue explicar a maioria das causas.

### Precisa melhorar
Depende de tentativa e erro, não coleta evidências ou corrige sem determinar causa.

## Pergunta final

“Um usuário informa que não consegue acessar um servidor remoto. Como investigar?”

Passe por:

~~~text
Escopo → L1 → L2 → L3 → DNS → TCP → Aplicação
~~~

Não pule diretamente para “reiniciar o equipamento”.
