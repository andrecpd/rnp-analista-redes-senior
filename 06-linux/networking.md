# Linux — Redes e Troubleshooting

## Comandos essenciais
```bash
ip addr
ip link
ip route
ip neigh
ss -lntup
ping <ip>
traceroute <ip>
tracepath <ip>
dig <nome>
nslookup <nome>
curl -I https://example.com
tcpdump -ni any host 10.0.0.1
mtr <ip>
```

## Roteamento
```bash
ip route
ip route get 8.8.8.8
```

## DNS
```bash
dig A example.com
dig AAAA example.com
dig +trace example.com
```

## Captura
```bash
tcpdump -ni eth0 port 443
tcpdump -ni eth0 host 192.168.1.10
```

## Método
Não altere configuração antes de coletar evidências. Compare:
- interface
- IP/máscara
- gateway
- tabela de rotas
- ARP/NDP
- DNS
- portas/socket
- firewall
- captura de pacotes
