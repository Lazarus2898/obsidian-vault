# Footprinting
On older versions of Splunk, the default credentials are `admin:changeme`.
If the default credentials do not work, it is worth checking for common weak passwords such as `admin`, `Welcome`, `Welcome1`, `Password123`, etc.
Here we can see that Nmap identified the `Splunkd httpd` service on port 8000 and port 8089, the Splunk management port for communication with the Splunk REST API.

If able to log in, one can browse data, run reports, create dashboards and install applications. `https://10.129.201.50:8000/en-US/app/launcher/home`
Aside from this built-in functionality, Splunk has suffered from various public vulnerabilities over the years, such as this [SSRF](https://www.exploit-db.com/exploits/40895) that could be used to gain unauthorized access to the Splunk REST API.