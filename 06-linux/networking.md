# Linux — Redes e Troubleshooting RNP

## 1. Fluxo mental

`Interface → IP → Neighbor → Route → Socket → DNS → Firewall → Aplicação`

O objetivo não é decorar comandos isolados: é saber qual comando responde a cada hipótese.

## 2. Interfaces e endereçamento

```bash
ip addr
ip -br addr
ip link
ip -br link
ip link show eth0
```

Perguntas:
- interface está UP?
- existe endereço?
- prefixo está correto?
- há múltiplos endereços?
- há erro ou carrier problem?

## 3. Roteamento

```bash
ip route
ip -6 route
ip route get 8.8.8.8
ip -6 route get 2001:4860:4860::8888
```

**Pegadinha:** `ip route get` é excelente para descobrir a decisão de encaminhamento do kernel para um destino específico.

## 4. ARP/NDP

```bash
ip neigh
ip -6 neigh
```

Estados comuns:
- REACHABLE
- STALE
- DELAY
- PROBE
- FAILED

Se o gateway IP não é alcançável, compare:
`IP → tabela de rotas → neighbor → interface → VLAN/L2`.

## 5. Portas e sockets

```bash
ss -lntup
ss -nt
ss -plant
```

Use para responder:
- serviço está escutando?
- em qual porta?
- em qual endereço?
- conexão TCP está ESTABLISHED?
- existe processo associado?

### TCP: diagnóstico rápido

`LISTEN` = aplicação aguardando conexão.

`ESTAB` = sessão estabelecida.

`REFUSED` = host respondeu, mas não há serviço aceitando aquela conexão ou há rejeição ativa.

`TIME-WAIT` = estado normal após encerramento de conexões TCP; não significa automaticamente falha.

## 6. DNS

```bash
dig example.com
dig A example.com
dig AAAA example.com
dig +trace example.com
resolvectl status
```

Separe:
1. resolução DNS;
2. reachability IP;
3. conexão TCP;
4. TLS;
5. aplicação.

**Exemplo:** `dig` funciona e `curl` falha → DNS provavelmente não é a primeira hipótese.

## 7. Testes de aplicação

```bash
curl -I https://example.com
curl -v https://example.com
nc -vz 10.10.10.10 443
```

`curl -v` ajuda a separar resolução, conexão, TLS e HTTP.

## 8. Caminho e latência

```bash
ping -c 5 10.0.0.1
traceroute 10.0.0.1
tracepath 10.0.0.1
mtr -rw 10.0.0.1
```

**Atenção:** um hop isolado com perda/latência pode ser apenas rate-limit de ICMP. Observe se a perda continua até o destino.

## 9. Captura de pacotes

```bash
tcpdump -ni eth0
tcpdump -ni eth0 host 10.10.10.10
tcpdump -ni eth0 port 443
tcpdump -ni eth0 'tcp port 443'
tcpdump -ni eth0 icmp
tcpdump -ni eth0 -w captura.pcap
```

Método:
`Sintoma → filtro de captura → pacote esperado → pacote observado → conclusão`.

## 10. Sistema e serviços

```bash
systemctl status <servico>
journalctl -u <servico>
journalctl -xe
dmesg | tail
```

Use logs para correlacionar horário do incidente com eventos.

## 11. Troubleshooting sênior

### Host não acessa 10.10.20.10:443

```bash
ip -br addr
ip route get 10.10.20.10
ip neigh
ping -c 4 10.10.20.10
nc -vz 10.10.20.10 443
curl -vk https://10.10.20.10/
tcpdump -ni any host 10.10.20.10
```

A cada comando, formule uma pergunta. Evite executar comandos sem hipótese.

## 12. IPv6

```bash
ip -6 addr
ip -6 route
ip -6 neigh
ping -6 2001:db8::1
traceroute -6 2001:db8::1
tcpdump -ni eth0 icmp6
```

## 13. Pegadinhas

- `ping` testa ICMP, não uma aplicação TCP.
- `traceroute` não prova sozinho onde está a falha.
- DNS resolvido não significa TCP funcionando.
- Porta LISTEN não garante aplicação saudável.
- `TIME-WAIT` não é automaticamente erro.
- Capture pacotes quando o estado lógico não explica o sintoma.

## 14. Resumo de 30 segundos

`ip addr` = endereços

`ip route` = rotas

`ip neigh` = ARP/NDP

`ip route get` = decisão de encaminhamento

`ss` = sockets

`dig` = DNS

`curl/nc` = aplicação/TCP

`tcpdump` = evidência de pacote

`journalctl` = eventos

**Golden rule:** primeiro determine onde o pacote deveria estar; depois prove onde ele está parando.
