# Things to start with
`/robots.txt`
`/wp-admin`
`/wp-content`
`/wp-content/plugins`

### Enumeration
```bash
curl -s http://blog.inlanefreight.local | grep WordPress
curl -s http://blog.inlanefreight.local/ | grep themes

# Looking for the wpDiscuz
curl -s http://blog.inlanefreight.local/ | grep plugins
```

##### WPScan
```bash
sudo gem install wpscan

# Adding -t 5 for threads
# or enumerate ap
sudo wpscan --url http://blog.inlanefreight.local --enumerate --api-token dEOFB<SNIP>
```