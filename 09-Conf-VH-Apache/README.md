#  Configuração de VH com Apache - LiveLinux xbnet2.9

Exemplo simples de configuração de Virtual Host no Apache 2:

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

**Virtual Host Padrão**

Simplificando /etc/apache2/sites-available/000-default.conf

```
ServerName www.site.com
<VirtualHost *:80>
   DocumentRoot /var/www/
</VirtualHost>
```

Obs: o ServerName fora do VirtualHost significa que este é o domínio principal deste servidor Apache.

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

Criar uma página para um novo domínio no mesmo servidor de Apache

Criar um novo arquivo: /etc/apache2/sites-available/novosite.conf

```
<VirtualHost *:80>
   ServerName www.novosite.com
   DocumentRoot /var/www/novosite/
</VirtualHost>
```

*Obs: Criar a pasta e colocar o novo site em /var/www/novosite/index.html. Não esqueça de ativar o novo site no Apache e recarregar o servidor (reload).*

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-

Ativar sites e serviço do Apache:

Ativar um site:

`a2ensite novosite`

Desativar um site:

`a2dissite novosite`

Reiniciar o serviço do apache:

`/etc/init.d/apache2 restart`

Recarregar o serviço do apache (novos sites)

`/etc/init.d/apache2 reload`

-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x-x- 
