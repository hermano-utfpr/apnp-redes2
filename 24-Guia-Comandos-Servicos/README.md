# Comunicação de Serviços de Internet - LiveLinux xbnet2.9

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**Servidores**

Listar serviços, protocolos e portas:

`# netstat -tupan`

Verificar se o roteamento está ativo:

`# cat /proc/sys/net/ipv4/ip_forward`

ou

`# sysctl -a`

Ativar o roteamento (no caso servidor de firewall):

`# echo 1 > /proc/sys/net/ipv4/ip_forward`

ou

`# sysctl net.ipv4.ip_forward=1`

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**FTP - File Transfer Protocol**

Aplicação no servidor: ProFTPD

Lado cliente:

```
$ ftp [ip|dominio]
- usuário/senha
- help
- ls,get,put
```

ou

Usar navegador ou gerenciador de arquivos:

ftp://[usuario:senha]@[ip|dominio]

Exemplos:

```
$ ftp ftp.site.com.br
User:  fulano
Pass: abc123
> ls
  . ..
> put index.html
  ....
> exit
```

ou

ftp://ftp.site.com.br/imagens/

ftp://fulano:abc123@200.134.18.127

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**SSH - Secure SHell**

Aplicação no Servidor: OpenSSH

Lado cliente:

Acesso shell:

```
$ ssh [usuario]@[ip|dominio]
- [yes|no] aceitar ou não chave pública/certificado
- ls,ps,netstat,exit ... qualquer comando
```

Acesso file transfer:

```
$ sftp [usuario]@[ip|dominio]
- [yes|no] aceitar ou não chave pública/certificado
- ls,get,put,exit ... comandos ~ftp
```

Copiar diretamente:

`$ scp [usuario]@[ip|dominio]:/remoto/arquivo.txt /local/`

ou

`$ scp /local/arquivo.txt [usuario]@[ip|dominio]:/remoto/`

Exemplos:

```
$ ssh fulano@site.com.br
  [yes]
  ls
  ps aux
  logout
  exit
```

```
$ sftp fulano@site.com.br
  ls
  cd /var/www
  put index.html
```

`$ scp ico.jpg root@site.com.br:/var/www/imagens/`

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**Telnet - TErminal NETwork**

Lado servidor: Inetd

Lado cliente:

```
$ telnet [ip|dominio]
  usuario e senha
  ls,ps,cd,exit - diversos comandos
```

Exemplo:

```
$ telnet 200.134.18.199
     user: joao
     pass: abc124
  $ ls
  $ ps aux
  $ logout
  $ exit
```

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**SMTP - Simple Mail Transfer Protocol**

Aplicação no Servidor: Postfix

Lado cliente:

```
$ telnet [ip_mx|dominio_mx] [porta]
  -> helo,mail from,rcpt to,data,quit
```

Aplicativos: Mutt, Thunderbird, etc.

Exemplo:

```
$ telnet mail.com 25
  helo mail.com
  mail from: eu@teste.com
  rcpt to: voce@mail.com
  data
  Subject: Teste
  
  Teste de e-mail!

  .
  quit
```

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**DNS - Domain Name Service**

Aplicação no Servidor: Bind

Lado cliente:

```
$ dig @[ip|dominio] [-x] [query] [type] [-tcp]
- Sem @[ip|domino] consultará conforme /etc/resolv.conf
- (-x) se query fornecer endereço ip para reverso
- Query: domínio, fqdn ou ip reverso.
- Type: address (A), name server (NS), mail exchange (MX)
- (-tcp) utiliza tcp ao invés de udp
```

Configurar /etc/resolv.conf:

`   nameserver [ip]`

`$ ping dominio`

`$ host [ip|dominio]`

Exemplos:

```
$ dig @a.dns.br site.com.br NS
$ dig @ns1.site.com.br site.com.br MX
$ dig www.foxnews.com A -tcp
$ dig -x 200.221.2.45

$ host www.google.com
$ host 74.125.141.104
```

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**TFTP - Trivial FTP**

Aplicação Servidor: Inetd

Lado cliente:

```
$ tftp [ip|dominio]
  put arquivo
  get arquivo
```

Exemplo:

```
srv# mkdir /srv/tftp (conforme inetd.conf)
srv# touch /srv/tftp/arquivo.txt
srv# chmod 666 /srv/tftp/arquivo.txt

cli$ tftp servidor.com
     > put arquivo.txt
     > exit

srv# cat /srv/tftp/arquivo.txt
```

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**HTTP - Hyper-Text Transfer Protocol**

Aplicação no Servidor: Apache

Lado cliente:

`$ lynx http://[ip|dominio]`

`$ links http://[ip|domino]`

... navegadores web.

Via telnet:

```
$ telnet [ip|dominio] 80
  GET / HTTP/1.1
  Host: [dominio|ip]
```

Exemplo:

```
$ telnet www.site.com.br 80
GET / HTTP/1.1
Host: www.site.com.br


```

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**POP - Post Office Protocol**

Aplicação no Servidor: Courier

Lado cliente:

Usar Mutt, Thunderbird, ...

Via telnet:

```
$ telnet [ip|dominio] 110
  USER,PASS,LIST,RETR,DELE,QUIT
```

Exemplos:

```
$ telnet mail.linux.com 110
  USER torvalds
  PASS abc123
  LIST
  RETR 3
  DELE 3
  QUIT
```

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**IMAP - Internet Message Access Protocol**

Aplicação no Servidor: Courier

Lado cliente:

Usar Mutt, Thunderbird, ...

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x- 


