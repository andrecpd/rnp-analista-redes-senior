# MPLS e VRF

## MPLS
MPLS encaminha pacotes usando labels.

Conceitos:
- FEC
- Label
- LSR
- LER
- LSP

## LDP
Distribui labels associados às FECs.

Comandos Cisco típicos:
```text
show mpls ldp neighbor
show mpls forwarding-table
show mpls interfaces
```

## VRF
Mantém tabelas de roteamento separadas no mesmo equipamento.

Comandos:
```text
show vrf
show ip route vrf <nome>
```

## L3VPN
Em uma arquitetura MPLS L3VPN:
- PE mantém VRFs dos clientes.
- CE conecta cliente ao PE.
- MP-BGP transporta informações VPN.
- MPLS fornece transporte no core.

## Troubleshooting
1. CE-PE está up?
2. Rotas existem na VRF?
3. MP-BGP está estabelecido?
4. Labels/LSP estão corretos?
5. Next-hop é alcançável?
6. Há conflito de RD/RT/política?
