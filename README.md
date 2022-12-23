# Envia Backup do Mikrotik para Email e para o Telegram


## Comandos de Instalação

/tool e-mail set address=173.194.68.109 from="Nome do Seu Mikrotik" password=aaabbbcccdddzzzz port=587 start-tls=yes user=seu-email@gmail.com

/system script add dont-require-permissions=no name=Backup_Email owner=SEU-USER policy=ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon source=":log warning message="Click em System depois Script e altere o Backup_Email"

/system scheduler add interval=1d name=Backup_Email on-event=Backup_Email policy=ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon start-date=dez/23/2022 start-time=03:30:00


## Equipe BrLink.org
