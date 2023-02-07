# 42Course - Born2BeRoot
-VVC-

## *sudo*

### 1: Instalando *sudo*.
Mude para root com comando su.
```
$ su -
Password:
#
```
Instale sudo, via apt.
```
# apt install sudo
```

### Adicione usurario para o grupo *sudo*.
```
# adduser <username> sudo
```
>ou...
>```
># usermod -aG sudo <username>
>```
verifique usuarios do grupo *sudo*.
```
$ getent group sudo
```

### Rodando comandos via *sudo*.
```
$ sudo apt update
```

### Configurando *sudo*
```
$ sudo vi /etc/sudoers.d/<filename>
```
>ou alternativamente:
>```
>$ sudo vi /etc/sudoers
>```

Mudando arquivo sudores:
1. Tentativas de senha:
```
Defaults        passwd_tries=3
```
2. Messsagem customizada de erro:
```
Defaults        badpass_message="<custom-error-message>"
```
Para log do *sudo*
```
$ sudo mkdir /var/log/sudo

<~~~>
Defaults        logfile="/var/log/sudo/<filename>"
<~~~>
```

Para log de todas entradas e saidas do *sudo*:
```
Defaults        log_input,log_output
Defaults        iolog_dir="/var/log/sudo"
```
Para habilitar *TTY*:
```
Defaults       requiretty
```
Path do *sudo* - `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin`:
```
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
```

## SSH

### Instalando e configurando SSH:
Instale SSH:
```
$ sudo apt install openssh-server
```
Configure SSH:
```
$ sudo vi /etc/ssh/sshd_config
```
Setar porta 4242 para SSH:
```
Port 4242
```
Para desabilitar login como *root* no SSH:
```
#PermitRootLogin prohibit-password
```
Mude para:
```
PermitRootLogin no
```
Restart no SSH apos as modificacoes:
```
$ sudo service ssh restart
```
Verificar status do SSH:
```
$ sudo service ssh status
```

### Instalando e configurando UFW
```
$ sudo apt install ufw
```
Habiilitar Firewall.
```
$ sudo ufw enable
```
Liberar porta 4242 para firewall.
```
$ sudo ufw allow 4242
```
Checar status de firewall.
```
$ sudo ufw status
```

### Conectando ao servidor SSH
Para saber IP da MV:
```
$ ip a
```
Fora da dsua MV, use comando `ssh <username>@<ip-address> -p 4242`.
```
$ ssh <username>@<ip-address> -p 4242
```
Para fechar SSH.
```
$ logout
```
>ou...
>```
>>$ exit
>```

## Gerenciamento de Senhas fortes

### Para uma politica de senhas fortes

#### DUracao da senha
Configure `sudo vi /etc/login.defs`.
```
$ sudo vi /etc/login.defs
```
Setar a senha para terminar em 30 dias
```
PASS_MAX_DAYS   30
```
Setar o numero de dias minimo que pode ser feito a mudanca
```
PASS_MIN_DAYS   2
```
Mandar mensagem dizendo que a senha expira em 7 dias:
```
PASS_WARN_AGE   7
```

#### Senha forte
Instale *libpam-pwquality*.
```
$ sudo apt install libpam-pwquality
```
Configure a politica de senhas:
```
$ sudo vi /etc/pam.d/common-password
<~~~>
25 password        requisite           pam_pwquality.so retry=3
<~~~>
```
A linha deve ficar semelhante:
```
password        requisite              pam_pwquality.so retry=3 minlen=10 ucredit=-1 dcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root
```

### Criando novo usuario
Criar via sudo:
```
$ sudo adduser <username>
```
Verificar informacoes sobre a senha do usuario:
```
$ sudo chage -l <username>
Last password change					: <last-password-change-date>
Password expires					: <last-password-change-date + PASS_MAX_DAYS>
Password inactive					: never
Account expires						: never
Minimum number of days between password change		: <PASS_MIN_DAYS>
Maximum number of days between password change		: <PASS_MAX_DAYS>
Number of days of warning before password expires	: <PASS_WARN_AGE>
```

### Criando novo grupo
Criar grupo *user42*.
```
$ sudo addgroup user42
```
Adicione usuarios ao grupo:
```
$ sudo adduser <username> user42
```
Verifique os usuarios do grupo:
```
$ getent group user42
```

## *cron*

### Setar uma tarefa para se repetir a cada intervalo, usando *cron*
Configure tarefa para root, com cron:
```
$ sudo crontab -u root -e
```
Para agendar tarefa, mude a linha:
```
# m h  dom mon dow   command
```
para:
```
*/10 * * * * sh /path/to/script
```
No nosso exercicio, mudaremos para:
```
*/10 * * * * bash /<path>/monitoring.sh
```  
Checar as tarefas agendadas:
```
$ sudo crontab -u root -l
```

### Links para estudos:
https://www.vivaolinux.com.br/artigo/Instalacao-e-Configuracao-de-Servidor-SSH-no-Debian
https://www.edivaldobrito.com.br/como-ativar-o-sudo-no-debian/
https://brasilcloud.com.br/tutoriais/como-alterar-a-porta-de-acesso-ssh-no-linux/
https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-debian-9-pt
http://www.bosontreinamentos.com.br/linux-ubuntu/20-configurar-firewall-ufw-no-ubuntu-linux-parte-1/
https://itigic.com/pt/how-to-configure-a-strong-password-policy-in-debian/
https://pt.linux-console.net/?p=1754#gsc.tab=0
https://www.digitalocean.com/community/tutorials/how-to-edit-the-sudoers-file-pt
https://dev.to/womakerscode/tutorial-linux-terminal-verificando-informacoes-do-sistema-5h3
https://www.youtube.com/watch?v=pwCKvMIJNjE
https://www.youtube.com/watch?v=GGTbXq1FUI0
https://www.youtube.com/watch?v=PrVZVKxqHUc
https://www.youtube.com/watch?v=OZ1k0F3yJbo