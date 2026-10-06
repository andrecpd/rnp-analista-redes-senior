# LAB 06 — Linux Networking + Troubleshooting

## Topologia

~~~text
Linux-Client ---- Linux-Router ---- Linux-Server
~~~

## Comandos essenciais

### Interface

~~~bash
ip -br addr
ip link
ip addr show
~~~

### Roteamento

~~~bash
ip route
ip route get 8.8.8.8
~~~

### Vizinhança

~~~bash
ip neigh
~~~

### Portas

~~~bash
ss -lntup
~~~

### DNS

~~~bash
resolvectl status
dig example.com
dig @1.1.1.1 example.com
~~~

### Aplicação

~~~bash
curl -v http://<server>
curl -I https://example.com
~~~

### Captura

~~~bash
sudo tcpdump -ni any
sudo tcpdump -ni any host <IP>
sudo tcpdump -ni any port 53
sudo tcpdump -ni any tcp port 443
~~~

### Caminho

~~~bash
traceroute <IP>
mtr <IP>
~~~

## Servidor TCP

No Server:

~~~bash
python3 -m http.server 8080 --bind 0.0.0.0
~~~

No Client:

~~~bash
curl http://<server>:8080
~~~

## Falhas

### 1. Interface

~~~bash
sudo ip link set eth1 down
~~~

### 2. Rota

Remova a rota default e use ip route get.

### 3. DNS

Acesse por IP e por nome. Se IP funciona e nome não, investigue DNS.

### 4. TCP

Pare a aplicação e compare:

~~~bash
ss -lntup
curl -v
tcpdump
~~~

Identifique SYN, SYN/ACK e ACK.

### 5. MTU

~~~bash
ping -M do -s 1472 <destino>
~~~

Reduza o tamanho e compare.

## Método

1. Interface UP?
2. IP?
3. Vizinho?
4. Rota?
5. ICMP?
6. Porta TCP?
7. DNS?
8. Aplicação?
9. Firewall?
10. Pacote chega e volta?

## Desafio

Chamado: “Servidor está fora.”

Prove se é L1, L2, L3, DNS, TCP ou aplicação.
