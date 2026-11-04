### Directory
Using this tool for directory busting
```bash
fuff -w /path/to/wordlist:FUZZ -u http://IP:port/FUZZ
```
### Sub-Domain
```bash
fuff -w /path/to/wordlist:FUZZ -u http://FUZZ.IP.com/
```
### V-Host
```bash
ffuf -w /usr/share/seclists:FUZZ -u http://domain.com:port/ -H 'Host:FUZZ.domain.com'
```

### Page
```bash
fuff -w /path/to/wordlist:FUZZ -u http://IP:port/blog/indexFUZZ

or

fuff -w /path/to/wordlist:FUZZ -u http://IP:port/blog/FUZZ.php

# Or Recursion
fuff -w /path/to/wordlist:FUZZ -u http://IP:port/FUZZ -recursion -recursion-depth 3 -e .php -v
```

### SecLists
Is found in `/usr/share/seclists`