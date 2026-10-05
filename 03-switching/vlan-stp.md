# Switching — VLAN, Trunk e STP

## VLAN
Segmenta o domínio de broadcast em L2.

Access:
- normalmente carrega uma VLAN para o dispositivo final.

Trunk:
- transporta múltiplas VLANs entre equipamentos.
- 802.1Q adiciona tag às VLANs.

## STP
Objetivo: evitar loops L2.

Conceitos:
- Root Bridge
- Root Port
- Designated Port
- Alternate/Blocked
- Bridge ID
- Path Cost

RSTP converge mais rapidamente que STP tradicional.

## Troubleshooting
```text
show vlan brief
show interfaces trunk
show spanning-tree
show mac address-table
```

Perguntas:
- A VLAN existe?
- Está permitida no trunk?
- Native VLAN é compatível?
- A porta está up?
- Há loop?
- STP bloqueou a porta esperada?
- MAC está sendo aprendido na porta correta?
