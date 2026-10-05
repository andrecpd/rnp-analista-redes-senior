# Switching — VLAN, Trunk, STP/RSTP e Troubleshooting

## 1. Visão geral

Switching é a base da conectividade **Layer 2 (L2)**. Em uma prova de Analista de Redes Sênior, não basta saber o que é uma VLAN: é importante entender **domínio de broadcast, tabela MAC, trunk, 802.1Q, STP/RSTP, EtherChannel e o caminho de troubleshooting**.

Fluxo mental:

`Quadro Ethernet → MAC → VLAN → porta/trunk → STP → encaminhamento`

---

## 2. VLAN — Virtual LAN

Uma **VLAN** cria um domínio lógico de broadcast independente dentro da infraestrutura física.

Exemplo:

| VLAN | Uso | Sub-rede |
|---|---|---|
| 10 | Usuários | 192.168.10.0/24 |
| 20 | Servidores | 192.168.20.0/24 |
| 30 | Voz | 192.168.30.0/24 |
| 99 | Gerência | 192.168.99.0/24 |

### Ponto importante

**VLAN separa broadcast; não realiza roteamento entre VLANs.**

Para comunicação entre VLANs é necessário um dispositivo Layer 3, por exemplo:

- Switch L3 com SVI;
- Router-on-a-Stick;
- Roteador;
- Firewall.

---

## 3. Access x Trunk

### Access

Porta normalmente utilizada para conectar:

- PC;
- impressora;
- servidor;
- telefone IP;
- dispositivo final.

A porta é associada a uma VLAN de acesso.

Exemplo Cisco:

```text
interface Gi0/10
 switchport mode access
 switchport access vlan 10
```

### Trunk

Transporta múltiplas VLANs entre equipamentos.

Exemplo:

```text
interface Gi0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,99
```

### 802.1Q

O padrão **IEEE 802.1Q** identifica a VLAN através de uma tag inserida no quadro Ethernet.

Em uma questão de prova, atenção:

- Access → normalmente quadro não chega ao host com tag VLAN.
- Trunk → transporta múltiplas VLANs.
- VLAN nativa → é um conceito importante em trunks 802.1Q.
- Native VLAN incompatível entre os lados pode causar problemas e gerar alertas.

---

## 4. Native VLAN

A **Native VLAN** é a VLAN associada ao tráfego não tagueado em um trunk 802.1Q.

Boa prática:

- configurar explicitamente a native VLAN;
- manter a configuração consistente nos dois lados;
- evitar usar VLAN de usuários como native VLAN;
- permitir apenas as VLANs necessárias no trunk.

Exemplo:

```text
interface Gi0/1
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 10,20,30
```

**Pegadinha:** native VLAN mismatch não deve ser tratado simplesmente como "o trunk está down". O trunk pode permanecer operacional e ainda haver problemas de VLAN/tráfego.

---

## 5. Tabela MAC

O switch aprende o endereço MAC observando o **MAC de origem** dos quadros recebidos.

Exemplo:

```text
MAC 0011.2233.4455 → Gi0/10 → VLAN 10
```

Comandos:

```text
show mac address-table
show mac address-table dynamic
show mac address-table vlan 10
show mac address-table interface Gi0/10
```

### MAC flapping

Quando o mesmo MAC aparece alternadamente em portas diferentes, pode indicar:

- loop L2;
- conexão física indevida;
- bridge/switch conectado de forma incorreta;
- EtherChannel inconsistente;
- problema de STP;
- host virtualizado ou comportamento legítimo em alguns ambientes.

**Não conclua "loop" somente pelo alerta. Primeiro valide a evidência.**

---

# 6. STP — Spanning Tree Protocol

Objetivo principal:

> **Evitar loops de Layer 2.**

Loops L2 podem provocar:

- broadcast storm;
- MAC flapping;
- alta utilização das interfaces;
- CPU elevada;
- perda de conectividade;
- instabilidade da rede.

STP cria uma topologia lógica sem loops, colocando caminhos redundantes em estado que não encaminha tráfego.

---

## 7. Bridge ID e Root Bridge

A eleição do Root Bridge utiliza o **Bridge ID**.

De forma simplificada:

**menor Bridge ID vence.**

O Bridge ID envolve:

- prioridade;
- identificador relacionado à VLAN/MAC conforme implementação;
- endereço MAC.

Portanto, em uma questão:

> Qual switch tende a ser eleito Root Bridge?

Procure o equipamento com o **menor Bridge ID**.

Boa prática operacional: definir o root de forma planejada, em vez de deixar a eleição ocorrer por acaso.

---

## 8. Portas STP

### Root Port

É a porta do switch não-root que oferece o melhor caminho em direção ao Root Bridge.

Cada switch não-root normalmente possui uma Root Port por instância STP.

### Designated Port

É a porta escolhida para encaminhar tráfego em determinado segmento.

### Alternate/Blocked

É um caminho redundante que não encaminha tráfego enquanto a topologia atual permanecer válida.

**Pegadinha:** uma porta bloqueada pelo STP não significa necessariamente defeito. Ela pode estar bloqueada exatamente para impedir um loop.

---

## 9. Path Cost

STP compara o custo dos caminhos até o Root Bridge.

Regra prática:

> **Menor custo acumulado → melhor caminho.**

Se houver empate, entram outros critérios de desempate, como Bridge ID e Port ID.

---

# 10. STP x RSTP

| Característica | STP | RSTP |
|---|---|---|
| Padrão | 802.1D | 802.1w |
| Convergência | Mais lenta | Mais rápida |
| Objetivo | Evitar loops | Evitar loops com convergência mais rápida |
| Estados/roles | Tradicionais | Simplificados/melhorados |
| Uso atual | Legado em muitos ambientes | Muito comum |

RSTP reduz o tempo de recuperação após uma mudança de topologia.

---

# 11. EtherChannel / LACP

EtherChannel agrupa múltiplos links físicos em um único **Port-Channel lógico**.

Benefícios:

- maior capacidade agregada;
- redundância;
- operação lógica como um único enlace para STP.

LACP é definido pelo IEEE 802.3ad/802.1AX.

Comando Cisco:

```text
show etherchannel summary
```

### Problemas comuns

Verifique:

- velocidade/duplex;
- modo de negociação;
- VLANs permitidas;
- trunk/access;
- native VLAN;
- MTU;
- configuração consistente entre os membros.

**Pegadinha:** dois links físicos conectados entre switches não significam automaticamente que o tráfego será distribuído corretamente. O EtherChannel precisa estar formado de maneira consistente.

---

# 12. Troubleshooting — cenário de prova

## Cenário 1 — Usuário não acessa a rede

Sequência recomendada:

```text
1. Interface física
2. VLAN da porta
3. Tabela MAC
4. Trunk
5. VLAN permitida
6. STP
7. SVI/gateway
8. ARP
9. Roteamento
10. ACL/firewall
```

Comandos:

```text
show interfaces status
show interfaces Gi0/10
show vlan brief
show mac address-table interface Gi0/10
show interfaces trunk
show spanning-tree vlan 10
show ip interface brief
show ip route
show ip arp
```

---

## Cenário 2 — VLAN funciona em um switch, mas não em outro

Hipóteses principais:

- VLAN não existe no segundo switch;
- VLAN não está permitida no trunk;
- trunk configurado incorretamente;
- native VLAN inconsistente;
- STP bloqueando o caminho;
- problema de EtherChannel;
- SVI/gateway incorreto.

Comece pela comparação dos dois lados do enlace.

---

## Cenário 3 — MAC flapping

Procedimento:

```text
1. Identificar o MAC
2. Descobrir em quais portas aparece
3. Verificar VLAN
4. Verificar topologia
5. Verificar STP
6. Verificar EtherChannel
7. Inspecionar conexões físicas
```

Não altere STP ou desligue portas antes de entender o impacto e a causa provável.

---

# 13. Troubleshooting por evidência

Use sempre:

**Sintoma → Evidência → Hipótese → Teste → Causa raiz → Correção → Validação**

Exemplo:

> **Sintoma:** usuários da VLAN 20 perderam conectividade.

> **Evidência:** VLAN 20 existe no switch de acesso, mas não aparece na lista de VLANs permitidas no trunk.

> **Hipótese:** o trunk está filtrando a VLAN 20.

> **Teste:** `show interfaces trunk`.

> **Correção:** adicionar VLAN 20 ao allowed VLAN conforme mudança aprovada.

> **Validação:** testar conectividade e confirmar aprendizado MAC/ARP.

---

# 14. Comandos Cisco — Quick Reference

### VLAN

```text
show vlan brief
show vlan id 10
```

### Interfaces

```text
show interfaces status
show interfaces Gi0/10
show interfaces counters errors
```

### Trunk

```text
show interfaces trunk
show interfaces Gi0/1 switchport
```

### MAC

```text
show mac address-table
show mac address-table dynamic
show mac address-table vlan 10
```

### STP

```text
show spanning-tree
show spanning-tree vlan 10
show spanning-tree root
show spanning-tree blockedports
```

### EtherChannel

```text
show etherchannel summary
show etherchannel port-channel
```

---

# 15. Pegadinhas para a prova da RNP

1. **VLAN não é roteamento.**
2. **Trunk transporta múltiplas VLANs.**
3. **802.1Q é o padrão de VLAN tagging.**
4. **Native VLAN está relacionada ao tráfego não tagueado no trunk.**
5. **Menor Bridge ID → Root Bridge.**
6. **STP existe para evitar loops L2.**
7. **Porta bloqueada pelo STP pode estar funcionando corretamente.**
8. **Root Port é o melhor caminho para o Root Bridge.**
9. **MAC é aprendido pelo endereço MAC de origem.**
10. **MAC flapping é sintoma; não é automaticamente a causa.**
11. **RSTP converge mais rapidamente que STP tradicional.**
12. **EtherChannel transforma vários links físicos em um enlace lógico.**
13. **LACP é usado para negociação de agregação de links.**
14. **Trunk operacional não significa que todas as VLANs estão funcionando.**
15. **Antes de alterar configuração, colete evidências.**

---

# 16. Resumo de 30 segundos

**VLAN** segmenta domínios de broadcast.

**Access** conecta normalmente dispositivos finais a uma VLAN.

**Trunk** transporta múltiplas VLANs usando 802.1Q.

**Native VLAN** trata o tráfego não tagueado do trunk.

**STP** evita loops L2 através de uma topologia sem loops.

**Root Bridge** é eleito pelo menor Bridge ID.

**Root Port** é o melhor caminho de um switch não-root até o Root.

**RSTP** melhora a velocidade de convergência.

**EtherChannel/LACP** agrega links físicos em um Port-Channel.

No troubleshooting:

```text
Interface → VLAN → MAC → Trunk → STP
→ SVI/ARP → Roteamento → ACL/Firewall → Serviço
```

### Regra de ouro

> **Não corrija pelo sintoma. Colete evidências, formule hipóteses, teste, corrija e valide.**
