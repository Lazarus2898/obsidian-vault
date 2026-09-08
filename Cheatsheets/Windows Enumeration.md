# NetExec
```bash
nxc smb IP -u '' -p ''

# Edit if needed
nxc smb ip -u '' -p '' --shares | tail -n +4 | awk '{print $5}' > shares.txt

nxc smb ip -u '' -p '' --shares | awk 'NR>3 {print $5}' > shares.txt

netexec ldap target -u username -p password --trusted-for-delegation --password-not-required --admin-count --users --groups
```

```bash
--pass-pol // Password Policy
--shares   // Prints the shares
--users    // Prints the users on this machine
```

# Enum4linux-ng
```bash
enum4linux-ng IP 
enum4linux-ng IP -u '' -p ''
```
