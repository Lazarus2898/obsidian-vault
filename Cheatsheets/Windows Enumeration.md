# NetExec
```bash
nxc smb IP -u '' -p ''

# Edit if needed
nxc smb ip -u '' -p '' --shares | tail -n +4 | awk '{print $5}' > shares.txt
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
