# Troubleshooting de Redes

## Método em 8 passos
1. Definir o sintoma.
2. Determinar escopo e impacto.
3. Coletar evidências.
4. Formular hipóteses.
5. Testar da forma menos invasiva.
6. Aplicar correção controlada.
7. Validar serviço.
8. Documentar causa raiz e prevenção.

## Camadas
Física → L2 → L3 → Transporte → Aplicação.

## Exemplo: usuário sem acesso à aplicação
1. Interface física?
2. VLAN?
3. DHCP/IP?
4. Gateway?
5. ARP/NDP?
6. Rota?
7. ACL/firewall?
8. TCP/443?
9. DNS?
10. Aplicação?

## Perda de pacotes
Investigue:
- erros físicos
- congestionamento
- drops
- MTU
- QoS
- CPU
- rota assimétrica
- firewall/policer

## Alta latência
Compare origem, trânsito e destino. Use ping, traceroute/mtr e métricas de interface. Não conclua que o primeiro salto com aumento de latência é a causa: roteadores podem limitar ICMP sem afetar o tráfego encaminhado.
