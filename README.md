# Requisitos para o mkbkp.sh funcionar

Instale o sshpass e unzip
* apt install sshpass -y
* apt install unzip -y

Entre no arquivo mkbkp.sh e altere as variaveis de TOKEN e CHAT_ID do telegram, após é só rodar o comando como no exemplo abaixo e dê as permissões necessarias.
* chmod +x mkbkp.sh
* ./mkbkp.sh "IP" "USER" "SENHA"

O script irá acessar seu mikrotik e irá primeiramente coletar o nome do seu equipamento usado o comando "/system identity print", com isso ele irá acessar pela segunda vez gerando o arquivo de backup, e na terceira vez ele irá acessar novamente de scp para baixar o arquivo para uma pasta temporaria para daí compactar e após enviar via para o telegram. Por fim ele irá remover o arvivo que agora já não é mais necessário. 

# Caso queira adicionar a cron para agendar o backup automatico.
No exemplo irei por para rodar o script todos os dias as 20:00 horas!

* contrab -e

00 20 * * *	/home/admin/Backup/mikrotik/mkbkp.sh "IP" "USUARIO" "SENHA" &>/dev/null
