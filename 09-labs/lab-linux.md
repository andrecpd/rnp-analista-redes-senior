# Lab Linux Networking

## Objetivos
Praticar sem depender de equipamentos físicos.

### Exercícios
1. Criar endereço IP em interface de laboratório.
2. Consultar rota.
3. Consultar vizinhos.
4. Testar DNS.
5. Abrir conexão TCP.
6. Capturar tráfego.

## Comandos
```bash
ip addr
ip route
ip neigh
ss -lntup
dig example.com
curl -I https://example.com
tcpdump -ni any
```

## Pergunta
Explique a diferença entre:
- problema de DNS
- problema de rota
- problema TCP
- problema de aplicação
