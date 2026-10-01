### Discovery
```bash
curl -s http://drupal.inlanefreight.local | grep Drupal

Or through Nodes
http://drupal.inlanefreight.local/node/1
```
1. `Administrator`: This user has complete control over the Drupal website.
2. `Authenticated User`: These users can log in to the website and perform operations such as adding and editing articles based on their permissions.
3. `Anonymous`: All website visitors are designated as anonymous. By default, these users are only allowed to read posts.

### Enumeration
```bash
curl -s http://drupal.inlanefreight.local/CHANGELOG.txt
curl -s http://drupal-acc.inlanefreight.local/CHANGELOG.txt | grep -m2 ""

Or using Droopscan found in the Joomla section

droopescan scan drupal -u http://drupal.inlanefreight.local
```