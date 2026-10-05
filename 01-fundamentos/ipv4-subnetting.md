# IPv4 e Subnetting

## Conceitos
IPv4 possui 32 bits. Uma máscara divide endereço em rede e host.

Exemplos:
- /24 = 255.255.255.0 = 254 hosts utilizáveis.
- /25 = 255.255.255.128 = 126 hosts.
- /26 = 255.255.255.192 = 62 hosts.
- /27 = 255.255.255.224 = 30 hosts.
- /28 = 255.255.255.240 = 14 hosts.
- /29 = 255.255.255.248 = 6 hosts.
- /30 = 255.255.255.252 = 2 hosts.

Fórmula de hosts: 2^(bits de host) - 2, nos casos tradicionais.

## VLSM
Permite usar tamanhos diferentes de sub-rede conforme a necessidade.

## Checklist de prova
1. Identifique o prefixo.
2. Descubra a máscara.
3. Calcule o tamanho do bloco.
4. Encontre a rede.
5. Encontre broadcast.
6. Determine primeiro/último host.
7. Valide se os dois hosts pertencem à mesma rede.

## Exemplo
192.168.10.70/26
Bloco = 64.
Redes: .0, .64, .128, .192.
Logo:
Rede .64
Primeiro host .65
Último host .126
Broadcast .127
