# Configuração do Firewall IPTables - LiveLinux xbnet2.9

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**Firewall com IPTables:**

`# iptables -v`

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**Manutenção das Regras**

Listar:

`# iptables -L -n -v --line-numbers`
- (-L) listar
- (-n) não resolver domínio reverso
- (-v) apresenta detalhes (verbose)
- (--line-numbers) irá numerar cada regra a cada linha

Criar regras simples:

`# iptables [-A|-I] [INPUT|OUTPUT|FORWARD] [line-number] -s [IP] -j [ACCEPT|DROP]`
- (-A) irá adicionar a regra ao final da lista (append)
- (-I) irá adicionar a regra ao início da lista (initial)
- (line-number) se especificado, adicionar na lista conforme número da linha
- (INPUT) regras para datagramas que entram destinados para esta máquina
- (OUTPUT) regras para datagramas que saem originados a partir desta máquina
- (FORWARD) regras para datagramas que são encaminhados (roteamento) por esta máquina
- (-s) endereço ip de origem (source)
- (-j) ação
- (DROP|ACCEPT) descartar (DROP) ou aceitar (ACCEPT)

Exemplos:

```
# iptables -A FORWARD -s 10.0.0.45 -j DROP
# iptables -I FORWARD -s 10.0.0.47 -j ACCEPT
# iptables -I FORWARD 2 -s 10.0.0.48 -j DROP
```

Salvar regras em um arquivo:

`# iptables-save > nomearquivo.txt`

Restaurar regras a partir de um arquivo:

`# iptables-restore < nomearquivo.txt`

Remover todas as regras:

`# iptables -F`
- (-F) remover/limpar (flush)

Zerar contadores de todas as regras:

`# iptables -Z`
- (-Z) zero

Remover uma regra específica:

`# iptables -D [INPUT|OUTPUT|FORWARD] [line-number]`
- (-D) delete]

`# iptables -D FORWARD 2`

Alterar política padrão:

`# iptables -P [INPUT|OUTPUT|FORWARD] [ACCEPT|DROP]`
- (-P) política (policy)

`# iptables -P INPUT ACCEPT`

`# iptables -P FORWARD DROP`

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

Regras Customizadas

Criar regras:

`# iptables -[A|I] [INPUT|OUTPUT|FORWARD] -p [icmp|tcp|udp] -s [ip/mask] --sport [port] -d [ip/mask] --dport [port] -j [ACCEPT|DROP|REJECT|LOG]`
- (-A) irá adicionar a regra ao final da lista (append)
- (-I) irá adicionar a regra ao início da lista (initial)
- (INPUT) regras para datagramas que entram destinados para esta máquina
- (OUTPUT) regras para datagramas que saem originados partir desta máquina
- (FORWARD) regras para datagramas que são encaminhados (roteamento) por esta máquina
- (-p) se especificado: icmp, tcp ou udp
- (-s) se especificado: ip ou rede de origem do datagrama
- (--sport) se especificado: porta de origem (1-65535)
- (-d) se especificado: ip ou rede de destino do datagrama
- (--dport) se especificado: porta de destino (1-65535)
- (-j) ação
- (ACCEPT) aceitar o datagrama
- (DROP) descartar o datagrama
- (REJECT) descartar o datagrama e enviar datagrama icmp de aviso de erro
- (LOG) apenas registrar em /var/log/syslog e testar a regra conseguinte

Exemplos:

`# iptables -I INPUT -p tcp -s 10.0.0.0/8 --dport 22 -j DROP`

-> descartar acesso ssh nesta máquina que sejam oriundos da rede 10.0.0.0/8.

`# iptables -A FORWARD -s 200.0.0.55 -d 201.0.0.10 -j ACCEPT`

-> permitir encaminhar(roteamento) qualquer datagrama IP da origem 200.0.0.55 para o destino 201.0.0.10.

`# iptables -A OUTPUT -p icmp -d 192.168.10.30 -j LOG`

-> apenas registrar qualquer datagrama icmp saíndo desta máquina com destino 192.168.10.30.

Registrar log de um datagrama que foi descartado:

`# iptables -A [INPUT|OUTPUT|FORWARD] -s [IP] -j LOG --log-prefix "anotacao"`

`# iptables -A [INPUT|OUTPUT|FORWARD] -s [IP] -j DROP`

As regras nesta sequência irão registrar (LOG) e tomar ação (DROP), exemplo:

`# iptables -A FORWARD -s 10.0.0.0/24 -j LOG --log-prefix "Acesso bloqueado: "`

`# iptables -A FORWARD -s 10.0.0.0/24 -j DROP`

`# tail -f /var/log/syslog | grep "Acesso bloqueado"`

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

Regras Stateful

`# iptables [-A|-I] [INPUT|OUTPUT|FORWARD] -p [all|icmp|tcp|udp] -m state --state [NEW|ESTABLISHED|RELATED] -j [ACCEPT|DROP|REJECT|LOG]`
- (-m state --state) cria e/ou verifica tabela de estados para esses datagramas
- (NEW) datagrama cria uma entrada na tabela de estados
- (ESTABLISHED) verifica se o datagrama possui uma conexão/estado estabelecida
- (RELATED) verifica se existe uma conexão/estado relacionado (usado para permitir aviso de erro ICMP)

Exemplo:

Aceitar qualquer protocolo desde que esteja relacionado na tabela de estados:

```
# iptables -A FORWARD -p all -m state --state ESTABLISHED,RELATED -j ACCEPT
# iptables -A FORWARD -p icmp -m state --state NEW -s 10.0.0.2 -d 10.0.1.111 -j ACCEPT
# iptables -A FORWARD -p tcp --dport 22 --syn -m state --state NEW -s 10.0.0.2 -d 10.0.1.111 -j ACCEPT
```

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-
