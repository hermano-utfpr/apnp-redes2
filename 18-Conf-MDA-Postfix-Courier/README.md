# Configuração de Maildir Postfix+Courier

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**Courier**

Configuração do Courier. Verifique como a pasta Maildir foi configurada no Courier e altere se assim desejar:

-> /etc/courier/pop3d

-> /etc/courier/imapd

Encontre a seguinte configuração:

`MAILDIRPATH = Maildir`

Após a configuração, reinicie os serviços:

-> `/etc/init.d/courier-pop3 restart`

-> `/etc/init.d/courier-imap restart`

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**Postfix**

O Postfix não possui entrada para Maildir, suas configurações padrões irão gravar os e-mails dos usuários em: /var/mail/fulano.

Portanto, configure uma entrada de Maildir para o Postfix em:

-> /etc/postfix/main.cf

Adicione a seguinte configuração:

`home_mailbox = Maildir/`

Reinicie o serviço:

-> `/etc/init.d/postfix restart`

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

Conferir configurações

Verifique se os serviços estão rodando:

-> `netstat -tpan`

Postfix estará na porta 25 TCP (SMTP)

Courier estará nas portas 110 TCP (POP3) e 143 TCP (IMAP)

Caso queira verificar os arquivos de log:

-> /var/log/mail.log

-> /var/log/mail.err

-> /var/log/mail.info

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

Criar caixa de correio para novo usuário

Primeiro adicione um usuário ao sistema:

-> `adduser fulano`

Verifique:

-> `getent passwd`

Crie a caixa de correio:

-> `maildirmake /home/fulano/Maildir`

Altere o dono da caixa de correio:

-> `chown -R fulano.fulano /home/fulano/Maildir`

Verifique:

-> `ls -laR /home/fulano/Maildir`

Pronto, o usuário já pode receber e enviar e-mails pela sua caixa de correio.

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x- 
