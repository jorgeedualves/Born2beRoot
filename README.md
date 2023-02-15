# Born2beRoot
Projeto consiste em criar um servidor ssh em máquina virtual e instalar o sistema operacional, configurar sudo, firewall e ssh.

## Criando a VM

Utilizando o VirtualBox, você vai separar um espaço pra configurar sua VM.
Você deve pesquisar entre os CentOs e o Debian, e escolher o que mais te agrada.
Você deve saber a diferença entre eles, e explicar o motivo da sua escolha.

- **Explicar como uma máquina virtual funciona:**
- **Explicar a sua escolha de sistema operacional e as diferenças básicas entre CentOS e Debian:**
- **Explicar o propósito das máquinas virtuais**
- **Explicar a diferença entre Aptitude e Apt e o que é AppArmor (para quem escolheu Debian) ou o que são SELinux e DNF (CentOS)**

## 1 - Configuração inicial:
- Separar quantidade de memória RAM. (1GB é suficiente);
- Criar tipo de disco VDI, com alocação dinâmica;
- Na configuração do VirtualBox, escolher o modo Bridge Adapter;
- Instalar o ISO do sistema que você escolher;
- Fazer uma instalação sem interface gráfica (requisito obrigatório);
- Configurar o Hostname (login42 = **seu login da 42 + 42 no final**);
- ROOT (login42) e senha de acordo com a política de segurança definida no PDF;
- Sua senha deve ter pelo menos 10 caracteres. Deve conter uma letra maiúscula e um número. Além disso, não deve conter mais de 3 personagens idênticos;
- A senha não deve incluir o nome do usuário;
- A seguinte regra não se aplica à senha root: A senha deve ter pelo menos 7 caracteres que não fazem parte da senha anterior;
- Claro, sua senha root deve estar em conformidade com esta política;
- Escolher a opção LVM criptografado, pra formatar o disco;
- Optar por partição HOME separada e usar uma senha de 20 caracteres pra criptografia. Quando o particionamento finalizar, você vai poder excluir as partições LVM dentro da partição estendida criptografada e montar as partições da maneira que for desejada;
- Instalar apenas o servidor SSH e utilitários de sistema padrão (Não pode nada de interface gráfica);
- GRUB, sim. E selecione o disco pra ter opções de inicialização;

Você precisa pesquisar sobre Apt e Aptitude, e escolher um dos dois pra usar. Eu usei o Aptitude pois não conhecia, e achei bem interessante.
É importante que você saiba as diferenças entre eles, e qual o motivo da sua escolha.
Para instalar o Aptitude, usei o Apt (nativo do Debian) com o comando apt-get install aptitude
Muito importante também você conhecer o SELinux e APParmor , entender pra que serve e poder explicar

## 2 - instalar e configurando o SUDO:

Talvez você precise instalar o SUDO no seu sistema.
Use o apt ou aptitude pra isso e depois configure as permissões do arquivo /etc/sudoers.d.
Nesta parte você deve definir algumas configurações no arquivo, referente a caminhos permitidos no sudo, tentativas de senha, mensagem de erro ao usuário, caminho do log e tty

### Etapa 1: instalando Sudo

1. Mude para root e seu ambiente via **su -    ;**
2. instale sudo via **apt install sudo;**
3. Verifique se o sudo foi instalado com sucesso via **dpkg -l | grep sudo;**

### Etapa 2: Adding User to sudo Group

1. Adicionar usuário ao grupo sudo via **adduser <username> sudo**;
2. Como alternativa, adicione o usuário ao grupo sudo via **usermod -aG sudo <username>;**
3. Verifique se o usuário foi adicionado com sucesso ao grupo sudo por meio do **getent group sudo**;
4. Reinicie para que as alterações tenham efeito, faça login e verifique os poderes superiores via **sudo -v;**

### Etapa 3: Executar comandos com privilégios de root

- De agora em diante, execute comandos com privilégios de root por meio do prefixo **sudo**.

       Exemplo: **sudo apt update ;**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04910f09-f77b-4c1a-b7cc-30986a95cdbf/Untitled.png)

### Editando /etc/sudoers.tmp

1. Insira os defaults editando o arquivo com o comando **sudo visudo**
    
     `$ sudo visudo`
    
2. A autenticação usando sudo **deve ser limitada a 3 tentativas** no caso de uma **senha incorreta**
    
    `Defaults        passwd_tries=3`
    
3. Uma **mensagem personalizada** de sua escolha deve ser exibida se um erro devido a um erro a senha ocorre ao usar o sudo.
    
    `Defaults        badpass_message=""`
    
4. Cada **ação usando sudo deve ser arquivada**, tanto as entradas quanto as saídas. O **arquivo de log** deve ser salvo na pasta /var/log/sudo/pasta
    
    `Defaults        logfile="/var/log/sudo/sudo.log"`
    
5. O **modo TTY** deve ser ativado por motivos de segurança.
    
    `Defaults        requiretty`
    
6. Também por razões de segurança, **os caminhos** que podem ser usados pelo sudo **devem ser restritos**.
    
    `Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"`
    
7. Para arquivar **todas as entradas e saídas sudo** para:
    - `Defaults        log_input,log_output`
    - `Defaults        iolog_dir="/var/log/sudo"`

## 3 - Configurar SSH

**O SSH é um protocolo utilizado pra troca de dados entre cliente e servidor
remoto de forma segura e dinâmica.
Ele possibilita a comunicação criptografada através da rede permitindo acessar e fazer alterações em outro computador através do terminal.**

Verifique se o SSH já esta instalado, caso não esteja, instale. (Usei openssh-server já vem no Debian).
É muito importante que você saiba o que é SSH, como configurar e pra que serve.

### Installing & Configuring SSH

1. Para instalar o SSH utilize:
    
    `$ sudo apt install openssh-server`
    
2. Verifique se o SSH está instalado:
    
    `$ dpkg -l | grep ssh`
    
3. Configure SSH via **sudo vi** ou **sudo nano**  /etc/ssh/sshd_config.
    
    `$ sudo nano /etc/ssh/sshd_config`
    
4. Para configurar o SSH usando a porta 4242, **substitua** a **Port 22** para **Port 4242:**
    
    `$ Port 4242`
    
5. Para desativar o login SSH como root, independentemente do mecanismo de autenticação, substitua **PermitRootLogin prohibit-passwor para PermitRootLogin no:**
    
    `$ PermitRootLogin no`
    
6. Após editar o arquivo, reinicio o ssh:
    
    `$ service sshd restart`
    
7. Verifique se o SSH está funcionando:
    
    `$ sudo service ssh status` ou `$ systemctl ssh status`
    

## 4 - Configurar o UFW

1. Instalar o UFW, instalei pelo Aptitude. Você precisa saber o que é, pra que serve e como configurar.
    
    `$ sudo apt install ufw`
    
2. Verifique se o ufw foi instalado com sucesso
    
    `$ dpkg -l | grep ufw`
    
3. Habilitar firewall
    
    `$ sudo ufw enable`
    
4. Permitir conexões de entrada usando a porta 4242
    
    `$ sudo ufw allow 4242`
    
5. Esses comandos definem os padrões para negar conexões de entrada e permitir conexões de saída.
    
    `$ sudo ufw default deny incoming`
    `$ sudo ufw default allow outgoing`
    
6. Verifique o **status do UFW** por meio do **sudo ufw status**
    
    `$ sudo ufw status`
    

## 5 - Configurar a interface de rede

1. Indica que a interface enp0s3 será configurada manualmente
    
    `iface enp0s3 inet static`
    
2. Define o endereço de rede da interface 1
    - `address <your adress>`
        
       para achar o ip, usar o comando **ip address**
        
3. definir a máscara de rede
    
    `netmask 255.255.255.0`
    
4. define gateway/route (route)
    
    `gateway <gateway_do_address>`
    
5. Testando a conexão SSH do seu cmd
    - `ssh username@ip_address -p port`
        
        EX: `ssh joeduard@192.168.1.20 -p 4242`
        
        `exit`
    
## 6 - Política de proteção de senha

1. Edite o arquivo **login.defs**
    
    `$ nano /etc/login.defs`
    
2. A senha deve expirar a cada 30 dias:
    
    `PASS_MAX_DAYS 30`
    
3. O número mínimo de dias permitido antes da modificação de uma senha
ser definido como 2:
    
    `PASS_MIN_DAYS 2`
    
4. O usuário deve receber uma mensagem de aviso 7 dias antes de sua senha expirar:
    
    `PASS_WARN_AGE 7`
    
    Para **usuários já criados** referente a **expirar**:
    
    `$ chage 'username' -M 30`
    
    Para **usuários já criados** referente a **mínimo de dias permitidos**:
    
    `$ chage 'username' -m 2`
    
    Para **usuários já criados** referente a **mensagem de aviso**:
    
    `$ chage 'username' -W 7`
    
    ### Verificação de qualidade(força) de senha
    
    A ação deste módulo é solicitar ao usuário uma senha e verificar sua força em um dicionário do sistema e um conjunto de regras para identificar escolhas ruins.
    
    A primeira ação é solicitar uma única senha, verificar sua força e, se for considerada forte, solicitar a senha uma segunda vez.
    
    1. Para utilizar o modulo PAM instale o pacote:
        
        `apt-get install libpam-pwquality`
        
    2. verificar se Libpam-pwquality foi instalado com sucesso via:
        
         `dpkg -l | grep libpam-pwquality`
        
    3. configurar a política de força da senha via:
        
        `sudo nano /etc/pam.d/common-password`
        
    4. adicione os seguintes sinalizadores na linha   **password     requisite     pam_pwquality.so retry=3** :
        - `password requisite pam_pwquality.so retry=3 **minlen=10 ucredit=-1 dcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root**`
        - Política de senha forte: deve ter no **mínimo 10 caracteres**
                
                `**minlen=10**`
                
         - Com ao menos **1 letra maiúscula** e ao menos **1 número**
                
                `**ucredit=-1` `dcredit=-1`**
                
         - não deve conter mais de 3 caracteres idênticos consecutivos
                
                `**maxrepeat=3**`
                
            - Não aceita o nome do usuário na senha
                
                `**reject_username**`
                
            - A senha deve ter pelo menos 7 caracteres que não fazem parte da senha anterior
                
                `**difok=7**`
                
            - Aplica as mesmas politicas ao Root
                
                `**enforce_for_root**`
                  
        
        Essas Regras só valem pra **novos usuários**, os antigos devem usar o comando **chage** com as variações:
        
        `chage -M (max days)`
        
        `chage -m (min days)`
        
        `chage -W (warning days)`
        
        `chage -l`
        
    
    ## 7 - Configurar Usuário
    
    1. Para criar novo usuário usando **sudo**:
        
        `sudo adduser <username>`
        
    2. Verifique se foi instalado com sucesso via **getent:**
        
        `getent passwd <username>`
        
    3. verifique as informações de expiração da senha do usuário recém-criado via:
        
         `$ sudo chage -l | <username>`
        
    
    ## 8 - Criando novo grupo
    
    1. Criar um novo grupo de usuário 42 via:
        
        `$ sudo addgroup user42`
        
    2. Verificar se o grupo user42 foi adicionado com sucesso via:
        
        `$ getent group user42`
        
    3. Adicionar usuário ao grupo do usuário 42 via:
        
        `$ sudo adduser <username> user42`
        
    ## 9 - Cron
    
    1. Configurar cron como root via **sudo:**
        
        `$ sudo crontab -u rrot -e`
        
    2. Para agendar um script de shell para ser executado a cada 10 minutos, substitua a linha  **# m h dom mon dow command** para:
        
        `/10 * * * * sh /path/to/script`
        
    3. Verifique os cron jobs agendados do root por meio de **sudo**:
        
        `sudo crontab -u root -l`
        
    - **Sintaxe do Cron**
        - **Asterisco (*)** – define todos os parâmetros de agendamento
        - **Vírgula (,)** – mantém duas ou mais horas de execução em um único comando
        - **Hífen (-)** – determina o intervalo da hora ao definir vários tempos de execução de único comando.
        - **Barra inclinada (/)** – cria intervalos pré-determinados de tempo dentro de um intervalo de tempo específico
        - **Last (L)** – tem o propósito específico de determinar o último dia da semana do mês respectivo. Por exemplo, 3L significa a última quarta-feira.
        - **Weekday (W)** – determina o dia da semana mais próximo de um tempo dado. Por exemplo, 1W significa se o 1° for um sábado, o comando vai ser executado na segunda-feira (3°)
        - **Hash (#)** – para determinar o dia da semana, seguido por um número que varia de 1 a 5. Por exemplo, 1#2 significa a segunda segunda-feira. /
        - **Ponto de interrogação (?)** - serve pra deixar um espaço.
        - **Minute (Minuto)** – é o minuto em que o comando vai rodar, variando de 0 a 59
        - **Hour (Hora)** – é a hora em que o comando será executado, variando de 0 a 23.
        - **Day of the month (Dia do mês)** – é o dia do mês em que o comando vai rodar, variando de 1 a 31.
        - **Month (Mês)** – é o mês em que o comando será executado, variando de 1 a 12.
        - **Day of the week (Dia da semana)** – é o dia da semana que você quer que o comando rode, variando de 0 a 7.
        - Você pode precisar configurar alguma ação com menos de um minuto, e pra isso precisa usar o **sleep segundos**; e mais de uma linha de comando pra alcançar o tempo desejado.
    
    `/10 * * * * /bin/sleep $(last --time-format is reboot | head -1 | awk -F ":" '{printf} `
    sleep para da um delay de 10 minutos a partir do last boot
    

## 10 - Script

[**monitoring.sh**](http://monitoring.sh/) deve ser desenvolvido em **bash** e sempre ser capaz de exibir as seguintes informações:

- **cron**: gerenciador de tarefas do linux que nos permite executar comandos em um determinado momento. Podemos automatizar algumas tarefas apenas dizendo ao cron qual comando queremos executar em um momento específico.
- **wall:** comando usado pelo usuário root para enviar uma mensagem a todos os usuários atualmente conectados ao servidor
- grep [options] "expression" file: pesquisar padrões em um arquivo.
- -v: mostrar cada linha no arquivo "expression"
- awk [texto de ação padrão] : manipular texto

## Descrição do script:

1. **A arquitetura do seu sistema operacional e sua versão do kernel:**
    - `arch=$(uname -a)`
        
        variável: **arch recebe o valor de =$**
        
        **uname**  exibe todas as informações do sistema; 
        
        **-a** equivale a all
        
2. **O número de processadores físicos:"**
    - `cpu=$(cat /proc/cpuinfo | grep cpu\ cores | awk -F":" '{print $2}')`
        
        variável: **cpu recebe o valor de =$**
        
        **cat** para visualizar o conteúdo do arquivo no caminho **/proc/cpuinfo**; 
        
        **|** conectar a saída de comando com a entrada do outro (combina comando)
        
        **awk -F":"** estabelecido a separação padrão com dois pontos "**:**"
        
        (**'{print $2}'**    imprime o valor da 2ª coluna
        
3. **O número de processadores físicos**
    - `vcpu=$(cat /proc/cpuinfo | grep processor | wc -l)`
        
        variável: **vcpu recebe o valor de =$**
        
        **cat** para exibir o conteúdo do arquivo no caminho **/proc/cpuinfo**;
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **grep processor** onde estiver a palavra **processor (grep pesquisa padrões em um arquivo)**
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **wc** contar a linhas, palavras e caracteres de um arquivo
        
        **-l** conta linhas
        
4. **A memória atual disponível em seu servidor e sua taxa de utilização como um pré-ajuste**
    - `memu=$(free -m | grep Mem | awk -F" " '{print $3} {printf("/"} {printf $2} {printf("MB ")}{printf("(%.2f%%)", $3/$2 *100)}')`
        
        variável: **memu recebe o valor de =$**
        
        **free** exibe memória livre
        
        **-m** exibir os valores em Megabytes
        
         **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **grep Mem** para pegar apenas a memória e não o Swap também
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **awk -F" "** estabelecido a separação padrão com espaço
        
        **{print $3}** imprimir o valor da 3ª coluna
        
        **{printf"/"}** imprimir o caractere "**/**"
        
        **{printf $2}** imprimir o valor da **2ª coluna**
        
        **{printf("MB ")}** imprimir a string **"MB"**
        
        **{printf("(%.2f%%)" ,** imprimir com 2 de precisão e caractere % no final do
        
        $3/$2 *100)}') Valor da **3ª coluna** divido pela **2ª coluna** **vezes 100**
        
5. **A taxa de utilização atual de seus processadores como uma porcentagem**
    - `udisk=$(df -H --total | tail -1| awk -F" " '{print $3}')`
        
        variável: **udisk recebe o valor de =$**
        
        **df ("disk filesystem")** exibe informações sobre espaço livre e ocupado das partições do sistema
        
        **-H ("humam readable")** mostra o output em Mb e Gb
        
        **- -total** suprime entradas insignificantes
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **tail -1** exibir a última linha
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **awk -F" "** estabelecido a separação padrão com espaço
        
        **'{print $3}'** imprimir o valor da 3ª coluna (**usado**)
        
    - `tdisk=$(df -H --total | tail -1| awk -F" " '{print$2}')`
        
        variável **tdisk recebe o valor de =$**
        
        **df ("disk filesystem")** exibe informações sobre espaço livre e ocupado das partições do sistema
        
        **-H ("humam readable")** mostra o output em Mb e Gb
        
        **- -total** suprime entradas insignificantes
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **tail -1** exibir a última linha
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **awk -F" "** estabelecido a separação padrão com espaço
        
        **'{print $2}'** imprimir a 2ª coluna (**tamanho**)
        
    - `pdisk=$(df -H --total | tail -1| awk -F" " '{print$5}')`
        
        variável **pdisk recebe o valor de =$**
        
        **df -H  - -total**
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **tail -1** exibir a última linha
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **awk -F" "** estabelecido a separação padrão com espaço
        
        **'{print $5}'** imprimir o valor da 5ª coluna (**porcentagem**)
        
6. **A memória atual disponível em seu servidor e sua taxa de utilização como uma porcentagem**
    - `cpuload=$(top -bn1 | grep '^%Cpu' | cut -c 9- | xargs | awk '{printf("%.1f%%"), $1 + $3}')`
        
        variável: **cpuload recebe o valor de =$**
        
        **top -bn1  (top -** exibe processos dp Linux**) (b -** operação em modo bash**) (n** - é número de interações**)(1 é** Estados de CPU únicos / separados**)**
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **grep '^Cpu'** onde estiver a palavra **^Cpu (**o sinal **^** é para ver apenas as linhas iniciais com %CPU**)**
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **cut -c 9-** define caracteres a serem selecionados
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **xargs -** executa linhas de comando a partir de um input
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **awk '{printf("%.1f%%"), $1 + $3}'**
        
7. **A data e hora da última reinicialização. Se o LVM está ativo ou não**
    - `boot=$(who -b | awk -F" " '{printf $4} {printf(" ")}{printf $5}')`
        
        variável: **boot recebe o valor de =$**
        
        **who**            - printa informações sobre usuários ligados
        
        **-b**             - ****a hora do último boot
        
        **|**              - conectar a saída de comando com a entrada do outro) combina comando
        
        **awk -F" "**      - ****estabelecido a separação padrão com espaço
        
        **'{printf $4}**   -  ****imprimir o valor da 4ª coluna
        
        **{printf(" ")}**  - ****imprimir **"espaço"**
        
        **{printf $5}'**   - ****imprimir o valor da 5ª coluna
        
    - `lvm=$(lsblk | grep lvm | sed -n 1p | awk -F" " '{print $6}' | head -1)`
        
        variável: **lvm recebe o valor de =$**
        
        **lsblk -** exibe informações em formato de árvore sobre as partições do HD e outros dispositivos como pen drives
        
        **|**   - Conectar a saída de comando com a entrada do outro) combina comando
        
        **grep lvm**   - **** Onde estiver a palavra **lvm (grep pesquisa padrões em um arquivo)**
        
        **|**          - Conectar a saída de comando com a entrada do outro) combina comando
        
          **sed        -** É um editor de texto que recebe valores de entrada, armazena em um buffer, realiza os comandos no arquivo e depois exibe a saída
        
        **-n 1p         -** Retorna a primeira linha
        
        **|**            - Conectar a saída de comando com a entrada do outro) combina comando
        
        **awk -F" "**    - Estabelecido a separação padrão com espaço
        
        **'{print $6}'** - Imprimir o valor da 6ª coluna
        
        **|**             - Conectar a saída de comando com a entrada do outro) combina comando
        
        **head -1**       - Imprime a primeira linha do arquivo
        
    - `check="lvm"` variável **check** recebe a string **"lvm"** e o valor $
        
        `if [$lvm = $check ]; then` Se valor de **lvm** estiver ativo
        
        `use="yes"` ****
        
        `else` Se valor de **lvm** não estiver ativo
        
        `use="no"`
        
        `fi`
        
8. **O número de conexões ativas**
    - `ntcp=$(ss -s | awk -F" " '{print $4}' | cut -b 1 | sed -n 2p)`
        
        variável: **ntcp recebe o valor de =$**
        
        **ss  -** checa os sockets
        
        **-s** printa um resumo das estatísticas
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **awk -F" "** estabelecido a separação padrão com espaço
        
        **'{print $4}'** imprimir o valor da 4ª coluna
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **cut -b 1** remove seções de cada linha dos arquivos
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **sed -n 2p**
        
9. **O endereço IPv4 do seu servidor e seu endereço MAC (Media Access Control)**
    - `ip=$(ip a | grep inet | sed -n 3p | awk -F" " '{printf $2}' | cut -b -13)`
        
        variável: **ip recebe o valor de =$**
        
        **ip a** exibe a configuração de interfaces de rede
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **grep inet** procura o padrão **inet**
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **sed -n 3p** para filtrar e pegar a 3ª linha
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **awk -F" "** estabelecido a separação padrão com espaço
        
        **'{printf $2}'** imprimir o valor da 2ª coluna
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **cut -b -13 -** para deixar apenas o número do ip
        
    - `mac=$(ip a | grep -i ether | awk -F" " '{printf $2}')`
        
        variável: **mac recebe o valor de =$**
        
        **ip a** exibe a configuração de interfaces de rede
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **grep -i ether** para ignorar letras maiúsculas e minúsculas
        
        **|** conectar a saída de comando com a entrada do outro) combina comando
        
        **awk -F" "** estabelecido a separação padrão com espaço
        
        **'{printf $2}'** imprimir o valor da 2ª coluna
        
10. **O número de comandos executados com o programa sudo**
    - `seqc=$(cat /var/log/sudo/seq)`
        
       variável: **seqc recebe o valor de =$**
        
       **cat /var/log/sudo/seq**
        
    - `sudolog=$(echo "ibase=36;obase=A;$seqc" | bc)`
        
       variável: **sudolog recebe o valor de =$**
        
       **echo** exibir
        
       **"ibase=36;obase=A;$seqc"** transformar a entrada 36 para saída A
        
       **|** conectar a saída de comando com a entrada do outro) combina comando
        
       **bc** calculadora do sistema
11. 
