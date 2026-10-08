[GitHub vs. BitBucket vs. GitLab](https://stackshare.io/stackups/bitbucket-vs-github-vs-gitlab)

# Discovery
Going to the host page.
`http://gitlab.inlanefreight.local:8081/admin/application_settings/general`
If we can obtain user credentials from our OSINT, we may be able to log in to a GitLab instance. Two-factor authentication is disabled by default.
[Version of GitLab](https://www.exploit-db.com/exploits/49821)
### Signing In
`http://gitlab.inlanefreight.local:8081/users/sign_in`
Browsing the `/help` page will be the version of the `GitLab`.

`NOTE:` There have been a few serious exploits against GitLab [12.9.0](https://www.exploit-db.com/exploits/48431) and GitLab [11.4.7](https://www.exploit-db.com/exploits/49257) in the past few years as well as GitLab Community Edition [13.10.3](https://www.exploit-db.com/exploits/49821), [13.9.3](https://www.exploit-db.com/exploits/49944), and [13.10.2](https://www.exploit-db.com/exploits/49951)
### Not Signed In
Going to the `/explore`
Doing OSINT of the webpage allowing for any open repo's to gain access to any information.
Going to things such as `groups, snippets or help`.

### Getting In
If we try to register with an email that has already been taken, we will get the error `1 error prohibited this user from being saved: Email has already been taken`.

# Attacking GitLab
