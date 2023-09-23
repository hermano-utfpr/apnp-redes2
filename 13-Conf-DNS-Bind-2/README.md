# Configuração de DNS Master/Slave - LiveLinux xbnet2.9

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

Configuração de Exemplo DNS Master em 10.0.0.1

-> Arquivo /etc/bind/named.conf.local

```
zone "site.com" IN {
   type master;
   file "db.site.com";
   allow-transfer { 10.0.0.2; };
};
```

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

Configuração de Exemplo DNS Slave em 10.0.0.2

-> Arquivo /etc/bind/named.conf.local

```
zone "site.com" IN {
   type slave;
   file "db.site.com";
   masters { 10.0.0.1; };
};
```

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

Observações:
- O servidor DNS Slave também deve ser incluso como DNS de autoridade na configuração do domínio. Exemplo: `@ IN NS slave`.

- Se a configuração estiver correta, o arquivo /var/cache/bind/db.site.com será criado automaticamente no servidor DNS Slave.

- O arquivo do DNS Slave ficará no formato "raw", para converter:

`slave# named-compilezone -f raw -F text -o /tmp/db.site.com site.com /var/cache/bind/db.site.com`

`slave# cat /tmp/db.site.com`

Ou, alternativamente, colocar `masterfile-format text;` em named.conf.local no servidor DNS Slave.
