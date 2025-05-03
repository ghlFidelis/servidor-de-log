# servidor-de-log

Documentação - Configuração de Servidor de Log com Rsyslog - RedesBR
1. Introdução
Esta documentação descreve o processo de configuração de um servidor de log centralizado utilizando o serviço rsyslog em uma rede com três computadores, conforme a situação-problema apresentada pela empresa fictícia RedesBR. O objetivo é centralizar os arquivos de log para facilitar o monitoramento e a administração da rede.
2. Etapas da Configuração
2.1 Designação das Máquinas
Servidor de log: PC1
Clientes: PC2 e PC3
2.2 Configuração do Servidor (PC1)
1. Instalar o rsyslog:
sudo apt update
sudo apt install rsyslog
2. Editar o arquivo de configuração:
sudo nano /etc/rsyslog.conf
Descomentar ou adicionar as linhas abaixo:

module(load="imudp")
input(type="imudp" port="514")

module(load="imtcp")
input(type="imtcp" port="514")

3. Criar diretório personalizado para logs:
sudo mkdir /var/log/cliente-logs
Adicionar ao final do /etc/rsyslog.conf:

$template RemoteLogs,"/var/log/cliente-logs/%HOSTNAME%.log"
*.* ?RemoteLogs

4. Reiniciar o serviço rsyslog:
sudo systemctl restart rsyslog
2.3 Configuração dos Clientes (PC2 e PC3)
1. Instalar o rsyslog:
sudo apt update
sudo apt install rsyslog
2. Editar o arquivo de configuração:
sudo nano /etc/rsyslog.conf
Adicionar ao final do arquivo:
*.* @IP_DO_SERVIDOR:514
Substituir IP_DO_SERVIDOR pelo IP real do servidor (PC1).
3. Reiniciar o rsyslog:
sudo systemctl restart rsyslog
2.4 Verificação do Funcionamento
1. No servidor, listar arquivos de log:
ls /var/log/cliente-logs/
2. Testar envio de log a partir do cliente:
logger "Teste de log do PC2"
3. Verificar no servidor se o log foi recebido:
cat /var/log/cliente-logs/PC2.log
3. Possíveis Problemas e Soluções
Problema	Solução
Logs não chegam ao servidor	Verifique se a porta 514 está liberada no firewall (ex: ufw allow 514/udp)
Arquivos de log não aparecem	Confirme o IP do servidor no arquivo rsyslog.conf dos clientes
Erro ao usar logger	Verifique se o serviço rsyslog está ativo nos clientes
4. Conclusão
A configuração do servidor de log foi realizada com sucesso utilizando o rsyslog. Os computadores clientes estão enviando logs corretamente para o servidor, que os armazena em arquivos separados por nome de host. Essa centralização facilita o monitoramento e a auditoria da rede.
