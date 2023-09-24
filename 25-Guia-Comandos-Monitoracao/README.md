# Monitoração de Serviços da Internet

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**Monitoração e comunicação:**

Lado servidor:

`# tcpdump [-nXi] [interface] [filtro]`

`# netstat -tupan`

Lado cliente

`$ executar aplicativo`

ou

`$ nmap [-sS][-sU] [ip] [-p0-65535]`

Interceptar com o firewall

`# tcpdump [-nXi] [interface] [filtro]`

ou

`# iptstate`

Exemplo monitorando acesso PostgreSQL:

Lado servidor:

```
# netstat -tupan
  pgsql -> tcp -> 5432
# tcpdump -ni eth0 tcp port 5432
```

Lado cliente:
`$ nmap -sS 200.10.0.60 -p5432`

Interceptar com o firewall:

`# tcpdump -ni eth0 host 200.10.0.60 and tcp port 5432`

ou

`# iptstate`

-> irá capturar o estado de uma conexão pgsql

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**Filtros com TCPdump**

`# tcpdump [-nXi] [interface] [filtro]`
- (-n) não resolve nomes reversos
- (-X) conteúdo da mensagem em ASCII
- (-i) identificar interface para capturar

Filtros:
- (src) origem
- (dst) destino
- (host [ip]) um endereço IP
- (net [ip/mask]) um bloco de endereços IPs
- (port [port]) uma porta

Exemplos:

`# tcpdump -nXi eth0 udp port 69`

`# tcpdump -i eth0 icmp`

`# tcpdump -ni eth0 host 200.1.0.20 and port 80`

`# tcpdump -ni eth0 src host 187.0.0.7 and dst net 200.1.0.0/24 and dst port 22`

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**Mapeamento com NMap**

`$ nmap [opções] [ip|rede] [portas]`

Exemplos:

Varrer uma rede:

`$ nmap -n 200.134.100.0/24`

(-n não resolve reverso)

Varrer alguns IPs:

`$ nmap 200.134.100.4-8`

ou

`$ nmap 200.134.100.4,5,7`

Varrer e verificar portas TCP:

`$ nmap -sS 200.134.100.5 -p21,22,23`

Varrer e verificar portas UDP:

`$ nmap -sU 200.134.100.5 -p53`

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

