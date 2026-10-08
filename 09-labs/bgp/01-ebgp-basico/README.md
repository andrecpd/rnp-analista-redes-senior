# LAB-BGP-01 — eBGP básico entre dois ASNs

**Nível:** avançado (revisão prática) · **Tempo:** 60–90 min · **Plataformas:** GNS3 ou EVE-NG  
**Implementação principal:** FRRouting (FRR), software livre. Não requer licença Cisco.

## Objetivos

- Explicar o papel do ASN, Router ID, peer eBGP e TCP/179.
- Estabelecer uma sessão eBGP IPv4 entre dois roteadores.
- Anunciar um prefixo com `network` e confirmar os atributos recebidos.
- Distinguir a tabela BGP da tabela de roteamento do sistema.
- Diagnosticar erros de ASN, conectividade, prefixo ausente e next-hop.

## Cenário

```text
       AS 65001                          AS 65002
  LAN-A 192.0.2.0/24                LAN-B 198.51.100.0/24
          |                                  |
      +-------+      10.0.12.0/30        +-------+
      |  R1   |---------------------------|  R2   |
      | .12.1 |                           | .12.2 |
      +-------+                           +-------+
      Lo: 192.0.2.1/24                 Lo: 198.51.100.1/24
      Router-ID 1.1.1.1                Router-ID 2.2.2.2
```

> Os prefixos TEST-NET são reservados para documentação (RFC 5737); não os use como endereços públicos reais. Neste lab, as LANs são representadas por interfaces loopback.

## Plano de endereçamento

| Nó | Interface | Endereço | Função |
|---|---|---|---|
| R1 | lo | 192.0.2.1/24 | Prefixo local de AS 65001 |
| R1 | eth1 | 10.0.12.1/30 | Enlace eBGP |
| R2 | eth1 | 10.0.12.2/30 | Enlace eBGP |
| R2 | lo | 198.51.100.1/24 | Prefixo local de AS 65002 |

Os nomes de interface podem variar no appliance. Confirme com `ip -br address` antes de aplicar os arquivos.

## Pré-requisitos

1. Crie dois nós Linux com FRR instalado e os serviços `zebra` e `bgpd` habilitados.
2. Conecte `eth1` de R1 diretamente a `eth1` de R2.
3. Configure endereços nas interfaces e loopbacks conforme a tabela. Em FRR/Linux, a interface `lo` precisa ter o endereço local configurado no sistema operacional.
4. Garanta que os roteadores consigam fazer ping entre 10.0.12.1 e 10.0.12.2 antes de configurar BGP.
5. Faça snapshot/checkpoint antes dos testes de falha.

Documentação: [FRR BGP User Guide](https://docs.frrouting.org/en/stable-10.2/bgp.html) · [GNS3 Docs](https://docs.gns3.com/) · [EVE-NG](https://www.eve-ng.net/).

## Etapa 1 — Configurar endereços no Linux

Em cada nó, ajuste o nome da interface se necessário.

**R1:**
```bash
sudo ip addr add 10.0.12.1/30 dev eth1
sudo ip link set eth1 up
sudo ip addr add 192.0.2.1/24 dev lo
ip -br addr
ping -c 3 10.0.12.2
```

**R2:**
```bash
sudo ip addr add 10.0.12.2/30 dev eth1
sudo ip link set eth1 up
sudo ip addr add 198.51.100.1/24 dev lo
ip -br addr
ping -c 3 10.0.12.1
```

Os comandos `ip addr add` não são necessariamente persistentes após reiniciar. Para o exercício, mantenha os nós ligados ou configure persistência conforme a distribuição.

## Etapa 2 — Aplicar FRR

Os arquivos deste diretório são exemplos de configuração integrada do FRR: [R1/frr.conf](R1/frr.conf) e [R2/frr.conf](R2/frr.conf). Faça backup do arquivo existente antes de substituir `/etc/frr/frr.conf`. Ajuste permissões conforme a instalação e recarregue o serviço FRR.

Exemplo de aplicação, depois de copiar o arquivo correto para cada roteador:
```bash
sudo vtysh -f /etc/frr/frr.conf
sudo vtysh -c 'show running-config'
```

Se a instalação usar serviços systemd, valide com `sudo systemctl status frr`. Não reinicie serviços em equipamentos compartilhados ou de produção.

## Etapa 3 — Validar sessão e rotas

Execute em ambos os roteadores:
```text
show bgp summary
show bgp ipv4 unicast
show ip route
show running-config
show bgp neighbors
```

Critérios de sucesso:
- O peer aparece como **Established** em `show bgp summary`.
- R1 recebe `198.51.100.0/24` com AS_PATH contendo `65002`.
- R2 recebe `192.0.2.0/24` com AS_PATH contendo `65001`.
- A rota remota é elegível e instalada na tabela de roteamento.
- Com o cenário base, o ping entre as loopbacks deve funcionar após as rotas serem instaladas.

**Atenção:** o comando BGP `network` anuncia somente um prefixo que exista na tabela de roteamento local com o comprimento exato esperado.

## Etapa 4 — Captura de pacotes

No enlace entre R1 e R2, inicie uma captura no Wireshark com:
```text
tcp.port == 179
```
Observe o TCP handshake, depois as mensagens BGP OPEN, KEEPALIVE e UPDATE. Se a sessão não estabelecer, confirme primeiro a conectividade IP e se TCP/179 está permitido.

## Etapa 5 — Falhas controladas

Faça uma alteração por vez e registre o resultado antes de corrigir:

1. **ASN remoto incorreto:** altere temporariamente o `remote-as` em R1 para `65003`. Observe os logs e o estado da sessão; restaure para `65002`.
2. **Prefixo não anunciado:** remova temporariamente `network 192.0.2.0/24` em R1. Confirme que a sessão continua Established, mas o prefixo deixa de ser anunciado.
3. **Prefixo não existe na RIB local:** remova o endereço de loopback `192.0.2.1/24` de R1 e verifique se o anúncio deixa de ocorrer; restaure-o.
4. **Conectividade TCP bloqueada:** se sua plataforma permitir ACL/firewall, bloqueie TCP/179 em um único sentido. Registre a queda da sessão e remova a regra após o teste.

Nunca aplique essas falhas em roteadores de produção.

## Checklist de evidências

- [ ] Captura ou saída de `show bgp summary` dos dois peers.
- [ ] `show bgp ipv4 unicast` mostrando os prefixos e AS_PATH.
- [ ] `show ip route` mostrando a rota remota.
- [ ] Ping de loopback para loopback funcionando.
- [ ] Registro de pelo menos duas falhas: sintoma, hipótese, teste, causa raiz e correção.

Use o modelo [troubleshooting.md](troubleshooting.md) e confira o [gabarito comentado](gabarito.md) somente depois de tentar resolver sozinho.

## Perguntas de prova

1. Qual é a diferença entre eBGP e iBGP?
2. Qual transporte e porta o BGP utiliza por padrão?
3. O que significa o estado Established?
4. Por que uma sessão pode estar Established sem receber o prefixo esperado?
5. Qual é a função do AS_PATH e como ele ajuda a prevenir loops entre AS?
6. Por que uma rota pode aparecer na tabela BGP e não ser instalada na RIB/FIB?
7. Quais evidências coletaria antes de alterar a configuração?

## Entrega no GitHub

Crie um diretório de evidências (sem capturas que contenham dados sensíveis) e registre:
- Topologia e tabela de endereçamento.
- Configurações finais sanitizadas.
- Saídas dos comandos de validação.
- Relatório de troubleshooting e respostas às perguntas.

**Conclusão esperada:** sessão eBGP operacional, anúncio bidirecional de prefixos e capacidade de explicar a causa das falhas introduzidas.
