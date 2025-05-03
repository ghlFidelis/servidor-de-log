# servidor-de-log

Configuração de Servidor de Log com Rsyslog - RedesBR

Introdução  
Esta documentação explica como configurar um servidor de log centralizado usando o serviço rsyslog. A proposta é da empresa fictícia RedesBR, que deseja centralizar os logs de três computadores da rede para facilitar o monitoramento.  
Designação das Máquinas  
- Servidor de log: PC1  
- Clientes: PC2 e PC3  
Configuração do Servidor (PC1)  
Instalação do rsyslog  
sudo apt update  
sudo apt install rsyslog  
Edição do arquivo de configuração  
sudo nano /etc/rsyslog.conf  
Dentro do arquivo, descomente ou adicione as seguintes linhas para permitir o recebimento de logs por UDP e TCP na porta 514:  

module(load="imudp")  
input(type="imudp" port="514")  

module(load="imtcp")  
input(type="imtcp" port="514")  
Criar diretório para armazenar os logs recebidos dos clientes  
sudo mkdir /var/log/cliente-logs  
Ao final do mesmo arquivo /etc/rsyslog.conf, adicione:  

$template RemoteLogs,"/var/log/cliente-logs/%HOSTNAME%.log"  
*.* ?RemoteLogs  
Reiniciar o serviço rsyslog  
sudo systemctl restart rsyslog  
Configuração dos Clientes (PC2 e PC3)  
Instalação do rsyslog  
sudo apt update  
sudo apt install rsyslog  
Edição do arquivo de configuração  
sudo nano /etc/rsyslog.conf  
Adicione ao final do arquivo a linha abaixo para enviar os logs para o servidor. Substitua IP_DO_SERVIDOR pelo IP real do PC1:  

*.* @IP_DO_SERVIDOR:514  
Reiniciar o serviço rsyslog  
sudo systemctl restart rsyslog  
Verificação do Funcionamento  
Verificar no servidor se os arquivos de log foram criados  
ls /var/log/cliente-logs/  
Enviar uma mensagem de log de teste do cliente  
logger "Teste de log do PC2"  
Checar se o log foi recebido no servidor  
cat /var/log/cliente-logs/PC2.log  
Possíveis Problemas e Soluções  

Problema  
Logs não chegam ao servidor  
Arquivos de log não aparecem  
Solução  
Verifique se a porta 514 está liberada no firewall (ufw allow 514/udp)  
Confirme se o IP do servidor está correto no arquivo rsyslog.conf do cliente  
Verifique se o serviço rsyslog está ativo no cliente  
Conclusão  
A configuração do servidor de log foi concluída com sucesso. Os clientes estão enviando logs corretamente para o servidor, que os armazena em arquivos separados com base no nome da máquina. Isso torna o monitoramento e a auditoria de eventos da rede mais organizada e eficiente.
