# Perguntas de Entrevista — Analista de Redes Sênior

## Como responder

Use **Contexto → Evidência → Decisão → Ação → Validação → Prevenção**.

### Routing / BGP / OSPF

1. Como desenharia um backbone redundante com BGP e OSPF?
2. Quando usaria OSPF e quando usaria BGP?
3. Como investigaria um BGP Established sem prefixo?
4. Como analisaria o best-path BGP?
5. Como aplicaria Local Preference, MED e communities?
6. Como evitaria loops BGP?
7. Qual a diferença entre eBGP e iBGP?
8. O que faria se OSPF ficasse em ExStart?
9. Por que OSPF pode ficar em 2-Way em uma LAN?
10. Qual o papel de DR/BDR?

### Switching

11. Diferença entre access e trunk?
12. Como diagnosticar VLAN que não atravessa um trunk?
13. Como identificaria native VLAN mismatch?
14. Como investigaria MAC flapping?
15. Como escolheria o Root Bridge?
16. Quando usaria RSTP?
17. Como validaria EtherChannel/LACP?

### MPLS / VRF

18. Explique CE, PE, P, LDP e LSP.
19. Qual a diferença entre RD e RT?
20. Onde entra MP-BGP em uma L3VPN?
21. Como investigaria uma L3VPN sem comunicação?
22. Rota existe na VRF, mas o tráfego falha. O que verificaria?
23. Por que o roteador P não precisa conhecer todas as rotas dos clientes?

### IPv6

24. Como NDP substitui ARP?
25. Como funciona SLAAC?
26. Qual o papel de RS, RA, NS e NA?
27. Por que ICMPv6 é importante?
28. Como investigaria IPv6 com endereço, mas sem internet?
29. O que pode acontecer se PMTUD/ICMPv6 for bloqueado?

### Linux

30. Como descobrir a rota escolhida pelo Linux?
31. Como verificar se uma aplicação está escutando?
32. Como separar DNS de problema TCP?
33. Como capturar somente o tráfego de uma porta?
34. Quais comandos você usaria nos primeiros minutos de um incidente?

### Troubleshooting / postura sênior

35. Como diferencia causa raiz de sintoma?
36. Como reduz MTTR?
37. Como conduziria um incidente crítico?
38. Quando faria rollback?
39. Como evitaria mudança sem evidência?
40. Como documentaria uma causa raiz?
41. Como comunica risco técnico para gestão?
42. Como trata divergência entre equipes?
43. Como transforma incidente recorrente em melhoria?
44. Conte um incidente complexo que resolveu.
45. Como garante que a correção realmente resolveu o problema?

## Modelo de resposta forte

> "Primeiro eu delimitaria o impacto e coletaria evidências. Depois formularia hipóteses e testaria da menos invasiva para a mais invasiva. Com a causa raiz confirmada, aplicaria a correção ou rollback, validaria o serviço e registraria uma ação preventiva."

## Pegadinha de entrevista

Evite respostas como:

- "eu reinicio";
- "eu troco a configuração";
- "eu verifico tudo";
- "deve ser o BGP".

Prefira:

**"Eu verificaria X com Y. Se encontrar Z, então..."**

Isso demonstra raciocínio de nível sênior.
