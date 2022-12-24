# Envia Backup do Mikrotik para Email e para o Telegram
Este script foi criado para realizar o backup do Mikrotik e enviá-lo por e-mail e pelo aplicativo de mensagens Telegram.



## Pré-requisitos
Ter um bot do Telegram criado e o token de acesso.

Ter uma conta de e-mail com configuração de acesso SMTP.



## Instalação

1. Configure o acesso ao seu email através do comando `/tool e-mail set`:
```
/tool e-mail set address=SMTP_SERVER from="NOME_DO_SEU_MIKROTIK" password=SENHA_DO_EMAIL port=587 start-tls=yes user=SEU_EMAIL@gmail.com
```

2. Adicione o script ao Mikrotik através do comando `/system script add`:
```
/system script add name=backup_email owner=SEU_USER policy=ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon source=[SCRIPT_FONTE]
```

3. Adicione uma tarefa agendada para executar o script diariamente, por exemplo:
```
/system scheduler add interval=1d name=backup_email on-event=backup_email policy=ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon start-date=dez/23/2022 start-time=03:30:00
```


## Personalização
O script possui três variáveis que devem ser configuradas de acordo com suas necessidades:

`token`: Token de acesso do seu bot no Telegram.

`chatid`: ID do grupo ou usuário do Telegram para receber o backup.

`email`: Endereço de email para receber o backup.



# Equipe BrLink.org
