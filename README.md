![012](https://github.com/user-attachments/assets/8deaf5fc-c4d1-4489-8a9d-7f0eca3022eb)# Projeto-AWs-02
Atividade Compass UOl
# Projeto-Compass-UOL 
## RELATÓRIO DE INSTALAÇÃO DO SERVIDOR LINUX COM APACHE E NFS NA AWS
### Antonio Carvalho
##  (SET/2024)
## INTRODUÇÃO

Esta  documentação  tem  como  finalidade  demonstrar  e  guiar  o  passo  a  passo  para  a 
instalação de um servidor com o Sistema Operacional Linux2 AWS, no qual será instalado 
o  Server  Apache  (Hospeda  e  entrega  de  forma  assegura  e  eficiente,  os  conteúdos  da 
internet) e NFS (Network File System- documento fiscal ou a um protocolo de rede - Nota 
Fiscal de Serviços Eletrônica (NFS-e)) em um Servidor Cloud hospedado na Amazon AWS. 

### Parâmetros para a criação da Instância EC2: 
- Gerar uma chave pública para acesso ao ambiente;
- Criar instância EC2 em SO Amazon Linux:  2 AMI (HVM) - Kernel 5.10, SSD 
Volume Type;
- Instância EC2: Família t3.small (2vCPU, Memória 2GB);
- 16 GB SSD de Uso Geral (gp3);
- Gerar um Elastic IP e anexar à Instância EC2;
- Portas de comunicação  a serem liberadas ao acesso público: 22/TCP, 80/TCP, 
111/TCP e UDP, 443/TCP e 2049/TCP. 

# Passo 1:
## Criando uma Instância EC2

- No Console da AWS, clique em Launch Instance;
- Nome da Instância: Nomeie a instância;
  
![001](https://github.com/user-attachments/assets/2c2d16e5-0dc8-4f92-b5f4-19d24493c41e)

Escolher AMI (Amazon Machine Image):

![002](https://github.com/user-attachments/assets/d0428b3d-2a2c-48e9-8354-2e3011f0c06d)

- Amazon Linux 2 (64-bit);

![003](https://github.com/user-attachments/assets/c09afe41-19c4-4af3-b82c-576dd40741b3)

- Tipo de Instância: t3.small;

![004](https://github.com/user-attachments/assets/805782db-b7ba-4763-87ff-9d599034c2fa)

- Par de Chaves:Criado e baixado anteriormente chave.pem (automaticamente);

![005](https://github.com/user-attachments/assets/ee2e849e-601d-45fc-afe5-dfaffaab4753)


# Passo 2: 
## Configurando as portas: 

Crie um Security Group com as seguintes regras: 

- Acesso remoto 

22/TCP (SSH) 

- Tráfego web 

80/TCP (HTTP) 

- Conexões seguras 

443/TCP (HTTPS) 

- Cmunicação NFS 

111/TCP e UDP  

- NFS 

2049/TCP e UDP  

Habilitar as caixas  preencher com os dados solicitados e add respectivamente, 

![006](https://github.com/user-attachments/assets/1509df15-a80f-40c3-aac0-0bcc639d8e82)

# Passo 3: 
## Armazenamento: 
- Altere o armazenamento para  16 GB SSD (gp3). 
- Finalize a criação da instância EC2 em Launch Instance (Pronto Instância criada).

![008](https://github.com/user-attachments/assets/6199dc67-f65e-422e-8b16-a555ad9b0186)

# Passo 4: 
 
## Atribuindo um Elastic IP: 
- No Console EC2, vá para Elastic IPs no menu lateral;
- Clique em Allocate Elastic IP address;
- Selecione o Elastic IP recém-criado, clique em Actions > Associate Elastic IP
address;
- Associe o IP à instância EC2 criada;

![009](https://github.com/user-attachments/assets/899ea848-6752-4925-b005-f521dbb9df51)


# Passo 5:
 
## Liberando as Portas de Comunicação: 
 
## No Console EC2, acesse Security Groups no menu lateral. 
Selecione o Security Group associado à sua instância. 
## Na aba Inbound rules, edite para garantir as seguintes regras: 
- 22/TCP – Acesso SSH. 
- 80/TCP – Acesso HTTP. 
- 443/TCP – Acesso HTTPS. 
- 111/TCP e UDP – Comunicação NFS. 
- 2049/TCP e UDP – Comunicação NFS. 

![010](https://github.com/user-attachments/assets/f66cc24a-7820-49c2-a4ba-bd26bae0445b)


# Passo 6: 
 
## Conectando-se à Instância: 
 
### No Console EC2, vá para Instances. 
- Selecione a instância criada e clique em Connect; 
- Use EC2 Instance Connect para acessar via navegador.

![011](https://github.com/user-attachments/assets/4e059948-13e8-4fed-8dde-18ce41a86cf7)


# PROCEDIMENTO DE INSTALAÇÃO DO NFS E DO APACHE NA AWS 
# Passo 1: 
Conectando-se à Instância EC2 via SSH e atualizando o sistema com as respectivas 
descrições, lembrando que todo procedimento será desenvolvido no *console (Terminal)* 
do Sist. Operacional Amazon Linux 2:
- sudo yum update -y 

# Passo 2: 
## Instalando o NFS (Network File System): 
- sudo yum install nfs-utils –y
  
# Passo 3: 
## Configurando o NFS: 
### Criando o diretório a ser compartilhado (nomear o diretório): 
sudo mkdir -p /home/antonio/server-nfs 
Configurando as permissões do diretório: 
- sudo chown -R ec2-user:ec2-user /home/antonio/server-nfs/
- sudo chmod 755 /home/antonio/server-nfs/
  
### Editando o arquivo de configuração: /etc/exports 
- sudo nano /etc/exports 

### Para permitir o acesso a todos os clientes: 
- /home/antonio/server-nfs/ *(rw,sync,no_subtree_check) 
### Aplicando as mudanças: 
- sudo exportfs -a 
### Inicie e habilite o serviço NFS: 
#### Para Amazon Linux 2: 
- sudo systemctl start nfs-server
- sudo systemctl enable nfs-server
  
# Passo 4: 
#### Para Amazon Linux 2: 
- sudo systemctl status nfs-server 

# Passo 5: 
### Observe as regras de segurança da AWS para a porta 2049 (NFS), seguindo os procedimentos abaixo descrito. 
Através do console gráfico da AWS, acesse o Console da AWS, navegue para "EC2":

![012](https://github.com/user-attachments/assets/972cf987-47ad-4d80-86c2-60885a5f7a0a)


- No painel de serviços, selecione EC2. 
- No painel de navegação à esquerda, selecione Security Groups (Grupos de 
Segurança). 
#### Escolha o grupo de segurança associado à sua instância EC2 ou ao recurso que utiliza 
NFS. 
### Na aba Inbound Rules (Regras de Entrada): 
- Verifique se há uma regra que permite o tráfego de entrada na porta 
2049. 
- Certifique-se de que o protocolo seja TCP e revise as origens permitidas 
(como sub-redes específicas ou endereços IP). 
Na aba Outbound Rules (Regras de Saída): 
- Verifique se há uma regra que permite o tráfego de saída na porta 2049, se 
necessário.

## Usando a AWS CLI: 
Para listar as regras de entrada de um grupo de segurança específico, pode ser usado o seguinte comando: 

- aws ec2 describe-security-groups --group-ids < sg-008f8f46f3bd0c0a6> --query 
"SecurityGroups[*].IpPermissions"

Obs: É necessário substituir o <ID_DO_GRUPO_DE_SEGURANÇA> pelo ID do seu grupo de segurança. 

Verifique na saída do comando se há uma regra que permite o tráfego na porta *2049* 
para *NFS*. 
Se a porta *2049* não estiver listada ou configurada, pode ser criaada uma nova regra de 
entrada para permitir o tráfego adequado.

# Passo 6: 
### Para instalar o Apache, usar os comandos abaixo: 
- sudo yum install httpd -y 
- sudo systemctl start httpd 
- sudo systemctl enable httpd

# Passo 7: 
### Verifique o status do Apache: 
- sudo systemctl status httpd 

 ![013](https://github.com/user-attachments/assets/e6bf9d99-a529-424f-a2b6-88a1273fdce0)

 
# Passo 8: 
#### Deve ser observada as regras de segurança da AWS para as portas 80 (HTTP) e 443 (HTTPS). 

Obs: O procedimento segue o mesmo critério do passo 5, atentando as informações das 
respectivas portas. 
#### Verifique as regras de entrada (Inbound Rules): 
#### Na aba Inbound Rules (Regras de Entrada), verifique se há regras para: 
- Porta 80 (HTTP): Protocolo TCP com a porta 80 aberta para IPs públicos ou 
redes específicas. 
- Porta 443 (HTTPS): Protocolo TCP com a porta 443 aberta para 
IPs públicos ou redes específicas. 
#### Verifique as regras de saída (Outbound Rules) (opcional): 
#### Na aba Outbound Rules, certifique-se de que o tráfego de saída nas portas 80 e 443 esteja permitido, se necessário. 

![014](https://github.com/user-attachments/assets/7aa1426f-b0eb-491e-8b78-a3f77b06f1d5)

 
#### Usando a AWS CLI deve-se usar os comandos abaixo descrito: 
#### Listar as regras de segurança de um grupo específico: 
aws ec2 describe-security-groups --group-ids <ID_DO_GRUPO_DE_SEGURANÇA> --
query "SecurityGroups[*].IpPermissions" 

#### Verificar as portas 80 e 443:
{ 

    "IpProtocol": "tcp", 
    
    "FromPort": 80, 
    
    "ToPort": 80, 
    
    "IpRanges": [...] } 
    
 {   
 "IpProtocol": "tcp", 
 
    "FromPort": 443, 
    
    "ToPort": 443, 
    
    "IpRanges": [...] 
    } 
Caso as portas 80 e 443 não estejam listadas, pode –se adicionar regras para permitir o tráfego.  

# Passo 9: 
 Após os procedimentos anteriores, abra o navegador e digite o IP Público da instância 
EC2 para efetuar abertura da página padrão do Apache. 
 
# Passo 10: 
 
###MONITORAMENTO DOS SERVIÇOS:

1. Crie o script check_service.sh: 
2. #!/bin/bash 
3.  
4. #Configuração dos serviços e diretórios de saída 
5. SERVICES=("httpd" "nfs-server") 
6. NFS_PATH="/home/Antonio/server-nfs/logs" 
7. LOG_FILE="/var/log/service_monitor.log" 
8. ONLINE_FILE="$NFS_PATH/service_online.txt" 
9. OFFLINE_FILE="$NFS_PATH/service_offline.txt" 
10.   
11.  #Função para logar mensagens com timestamp atualizado 
12.  log_message() { 
13.      local message="$1" 
14.      local timestamp=$(date +"%Y-%m-%d %H:%M:%S") 
15.      echo "$timestamp - $message" >> "$LOG_FILE" 
16.  } 
17.   
18.  #Verifica se o diretório de montagem existe; caso contrário, tenta criar 
19.  if [ ! -d "$NFS_PATH" ]; then 
20.      log_message "Diretório $NFS_PATH não existe. Tentando criar..." 
21.      if mkdir -p "$NFS_PATH"; then 
22.          log_message "Diretório $NFS_PATH criado com sucesso." 
23.      else 
24.          log_message "Não foi possível criar o diretório $NFS_PATH. Abortando." 
25.          exit 1 
26.      fi 
27.  fi 
28.   
29.  #Monitoramento dos serviços 
30.  for SERVICE_NAME in "${SERVICES[@]}"; do 
31.      TIMESTAMP=$(date +"%Y-%m-%d %H:%M:%S") 
32.      if systemctl is-active --quiet "$SERVICE_NAME"; then 
33.          STATUS="ONLINE" 
34.          MESSAGE="O serviço $SERVICE_NAME está ONLINE." 
35.          OUTPUT_FILE="$ONLINE_FILE" 
36.      else 
37.          STATUS="OFFLINE" 
38.          MESSAGE="O serviço $SERVICE_NAME está OFFLINE." 
39.          OUTPUT_FILE="$OFFLINE_FILE" 
40.      fi 
41.   
42.      # Adiciona a informação ao arquivo de status apropriado 
43.      echo "$TIMESTAMP - Serviço: $SERVICE_NAME - Status: $STATUS - Mensagem: $MESSAGE" 
>> "$OUTPUT_FILE" 
44.   
45.      # Grava a mensagem no log principal 
46.      log_message "$MESSAGE" 
47.  done 
48.   
49.      #Registra no log principal que as listas foram atualizadas
>> 
50.  log_message "Lista de serviços ONLINE atualizada em $ONLINE_FILE."
    
     log_message "Lista de serviços OFFLINE atualizada em $OFFLINE_FILE." 

# Passo 11: 
#### Tornando o script executável e configure o cron: 
- sudo chmod +x $MONITOR_SCRIPTsudo nano /etc/crontab
  
#### Adicione a linha para configurar o cron para rodar o script a cada 5 minutos: 
- CRON_JOB="*/5 * * * * $MONITOR_SCRIPT" 
#### Adiciona o job ao crontab 
- (crontab -l 2>/dev/null; echo "$CRON_JOB") | crontab - 
#### Verifique os arquivos de saída e execute o script manualmente se necessário: 
- ./check_service.sh 
 
A configuração de monitoramento criada para ser executado a cada 5 minutos." 
#### Teste da página do Apache via IP: 

![016](https://github.com/user-attachments/assets/fd43a09b-d334-4bfc-b2bb-1c322b0e47e0)
























































