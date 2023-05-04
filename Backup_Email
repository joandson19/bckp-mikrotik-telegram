#!/bin/bash

# Coleta as inserções de  ip, user e senha
MK_IP=$1
MK_USER=$2
MK_PASS=$3

# Defina opções para o Telegram token e id do bot
TOKEN="TOKEN"
CHATID="CHAT_ID"

# Defina o formato de compactação desejado: "zip" ou "tar"
COMPAC="zip"

# Captura data do envio do backup
DATA=$(date +%d/%m/%Y)

# Cria uma pasta temporária para os backups
BACKUP_DIR=$(mktemp -d)

# Conecta-se ao Mikrotik via SSH
sshpass -p $MK_PASS ssh $MK_USER@$MK_IP "echo conectado via SSH no Mikrotik"
# Verifica se a conexão SSH foi bem sucedida 
if [ $? -ne 0 ]; then
  # Exibe mensagem de erro de conexão ao Mikrotik via SSH
  echo "Erro: não foi possível se conectar ao Mikrotik via SSH"
  exit 1
fi
# Coleta nome do Mikrotik via SSH
DEVICENAME=$(sshpass -p $MK_PASS ssh $MK_USER@$MK_IP  "/system identity print" | grep "name:" | cut -d " " -f 4)

# Remove quaisquer caracteres que não são letras, números, ou hifens da variável $DEVICENAME
DEVICENAME=$(echo "$DEVICENAME" | tr ' ' '-' | tr -cd '[:alnum:]-')
echo "device name: $DEVICENAME"

# Mensagens de notificação do Telegram
MSG="Mikrotik $DEVICENAME - Backup do dia $DATA"

# Define a variavel de nome do arquivo backup 
MK_BCKP_FILE="backup-$DEVICENAME-$(date +%Y%m%d).rsc"

# Gera arquivo Mikrotik
sshpass -p $MK_PASS ssh $MK_USER@$MK_IP "/export file=$MK_BCKP_FILE"

sleep 10s
# Faz o download do arquivo de dentro do Mikrotik
#sshpass -p $MK_PASS scp $MK_USER@$MK_IP:/$MK_BCKP_FILE .
sshpass -p $MK_PASS scp $MK_USER@$MK_IP:/"$MK_BCKP_FILE" "$BACKUP_DIR/"

# Verifica se o download foi bem sucedido
if [ $? -ne 0 ]; then
  # Exibe mensagem de erro ao fazer download do arquivo de backup
  echo "Erro ao fazer download do arquivo de backup"
  exit 1
fi

# Verifica se o arquivo de backup foi realmente baixado do Mikrotik
if [ ! -f /$BACKUP_DIR/$MK_BCKP_FILE ]; then
  # Exibe mensagem de erro informando que o backup não foi baixado 
  echo "Erro: arquivo de backup não foi baixado"
  exit 1
fi

# Apaga o arquivo dentro do Mikrotik para não ocupar espaço.
sshpass -p $MK_PASS ssh $MK_USER@$MK_IP "/file remove $MK_BCKP_FILE" 
echo "Apagando arquivo não mais necessario no mikrotik"
sleep 01

# Exibe uma mensagem de que o arquivo de backup foi baixado com sucesso
echo "Arquivo de backup baixado com sucesso!"

# Compacta o arquivo de backup e armazena na variável ARQ_BKP
if [ "$COMPAC" = "zip" ]; then
  zip -r "/$BACKUP_DIR/$MK_BCKP_FILE.zip" "/$BACKUP_DIR/$MK_BCKP_FILE"
  ARQ_BKP="$MK_BCKP_FILE.zip"
else
  tar -zcvf "/$BACKUP_DIR/$MK_BCKP_FILE.tar.gz" "/$BACKUP_DIR/$MK_BCKP_FILE"
  ARQ_BKP="$MK_BCKP_FILE.tar.gz"
fi
# Exibe uma mensagem de que o arquivo foi compactado
echo "Arquivo $ARQ_BKP compactado"

# Envia o backup baixado para o Telegram
curl -F document=@"/$BACKUP_DIR/$ARQ_BKP" -F caption="$MSG" "https://api.telegram.org/bot${TOKEN}/sendDocument?chat_id=$CHATID" &>/dev/null
echo "Arquivo enviado para o Telegram"

# Remove os arquivos temporarios

sleep 02
rm -rf "/$BACKUP_DIR"
