# Configuração DNS com Bind9 - LiveLinux xbnet2.9

Exemplos de configuração de serviços DNS:

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**Servidor DNS Cache**

Configurar servidor raiz
-> Arquivo /etc/bind/db.root

```
. IN NS root.com.
root.com. IN A 199.199.199.199
```

199.199.199.199 é o IP de um DNS Root

Permitir consulta de máquinas externas e desativar validação DNSSEC:
-> Modificar o arquivo /etc/bind/named.conf.options

```
...
   dnssec-validation no;
...
   allow-query { any; };
...
```
-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**Servidor DNS Master**

-> Arquivo /etc/bind/named.conf.local

```
zone "site.com" IN {
   type master;
   file "db.site.com";
};
```
-> Arquivo /var/cache/bind/db.site.com

```
$ORIGIN site.com.
$TTL 300;
@ IN SOA root admin (1 300 300 300 300);
@ IN NS root
root IN A 10.0.0.200

## Entradas adicionais:####
# @ IN A 10.0.0.201 # (resolve para http://site.com)
# @ IN MX 1 mail # (resolve para servidor Mail eXchange - relay)
# web IN A 10.0.0.201 # (resolve dominio para IP)
# dns IN CNAME root # (resolve de um nome para outro,
# www IN CNAME web  # canonical name)
# sub IN NS dns.sub # (delegar subdominio)
# dns.sub IN A 10.0.0.230 # 
# mail IN A 10.0.0.210 # 
```

-> Reiniciar o serviço DNS

`/etc/init.d/bind9 restart`

-> Verificar Logs:

`egrep site.com /var/log/syslog`

ou

`egrep named /var/log/syslog`

ou, ainda

`egrep "(site.com|0.0.10)" /var/log/syslog`

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**Servidor DNS Master REVERSO**

-> Arquivo /etc/bind/named.conf.local

```
zone "0.0.10.in-addr.arpa" IN {
   type master;
   file "db.0.0.10";
};
```

-> Arquivo /var/cache/bind/db.0.0.10

```
$ORIGIN 0.0.10.in-addr.arpa.
$TTL 300;
@ IN SOA admin.site.com. root.site.com. (1 300 300 300 300);
@ IN NS root.site.com.
200 IN PTR root.site.com.

## Entradas adicionais:####
# 201 IN PTR web.site.com. (resolve reverso de 10.0.0.201)
```
-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**Cliente DNS**

-> Arquivo /etc/resolv.conf

```
search site.com
nameserver 10.0.0.200
```

-> Consultas por Name Server

`dig @10.0.0.200 site.com NS`

-> Consultas por Address

`dig @10.0.0.200 root.site.com A`

-> Consultas por Reverso

`dig @10.0.0.200 -x 10.0.0.201`

ou

`host 10.0.0.201`

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x- 
