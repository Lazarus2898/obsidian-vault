# Discovery & Enumeration
Joomla collects some anonymous [usage statistics](https://developer.joomla.org/about/stats.html) such as the breakdown of Joomla, PHP and database versions and server operating systems in use on Joomla installations. This data can be queried via their public [API](https://developer.joomla.org/about/stats/api.html).

```bash
curl -s https://developer.joomla.org/stats/cms_version | python3 -m json.tool

# Discovery
curl -s http://dev.inlanefreight.local/ | grep Joomla
or 
/robots.txt
or
curl -s http://dev.inlanefreight.local/README.txt | head -n 5

# On certain Joomla installs you may be able to fingerprint the version from the Javascript files
curl -s http://dev.inlanefreight.local/administrator/manifests/files/joomla.xml | xmllint --format -
```
The `cache.xml` file can help to give us the approximate version. It is located at `plugins/system/cache/cache.xml`.

### Enumeration
```bash
# Possible version discovery
sudo pip3 install droopescan
droopescan scan joomla --url http://dev.inlanefreight.local/
```
[JoomlaScan](https://github.com/drego85/JoomlaScan)
```bash
python2 -m pip install bs4
python2 joomlascan.py -u http://dev.inlanefreight.local

# Credential discovery
git clone https://github.com/ajnik/joomla-bruteforce.git
sudo python3 joomla-brute.py -u http://dev.inlanefreight.local -w /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt -usr admin
```

# Attacking Joomla
During the Joomla enumeration phase and the general research hunting for company data, we may come across leaked credentials that we can use for our purposes. Using the credentials that we obtained in the examples from the last section, `admin:admin`, let's log in to the target backend at `http://dev.inlanefreight.local/administrator`. Once logged in, we can see many options available to us. For our purposes, we would like to add a snippet of PHP code to gain RCE. We can do this by customizing a template.

```bash
# Going to the TEMPLATES -> Configuration
```