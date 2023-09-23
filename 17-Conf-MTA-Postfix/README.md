#  Configuração mínima Postfix - LiveLinux xbnet2.9

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x

Configuração do Postfix para aceitar e-mails destinados para um domínio específico:

-> editar /etc/postfix/main.cf e adicionar o novo domínio:

```
...
myhostname = mail.empresa.net
mydomain = empresa.net
mydestination = localhost, localdomain, empresa.net
...
```

Observar:

myhostname: hostname do servidor MTA

mydomain: por padrão aceita localhost

mydestination: domínios os quais o MTA irá aceitar

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x

Configuração do Postfix para fazer relay (encaminhamento) de e-mails:

-> editar /etc/postfix/main.cf e adicionar a rede do remetente:

```
...
mynetworks = 127.0.0.0/8 10.0.0.0/8
...
```

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x 
