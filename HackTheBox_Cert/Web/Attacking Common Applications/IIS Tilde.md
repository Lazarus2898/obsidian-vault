IIS tilde directory enumeration is a technique utilised to uncover hidden files, directories, and short file names (aka the `8.3 format`) on some versions of Microsoft Internet Information Services (IIS) web servers. This method takes advantage of a specific vulnerability in IIS, resulting from how it manages short file names within its directories.

Assume the server contains a hidden directory named SecretDocuments. When a request is sent to `http://example.com/~s`, the server replies with a `200 OK` status code, revealing a directory with a short name beginning with "s". The enumeration process continues by appending more characters:

# Enumeration
`nmap -p- -sV -sC --open 10.129.224.91`
Manually sending HTTP requests for each letter of the alphabet can be a tedious process. Fortunately, there is a tool called `IIS-ShortName-Scanner` that can automate this task. You can find it on GitHub at the following link: [IIS-ShortName-Scanner](https://github.com/irsdl/IIS-ShortName-Scanner). To use `IIS-ShortName-Scanner`, you will need to install Oracle Java on either Pwnbox or your local VM.

`IIS-ShortName-Scanner`
`java -jar iis_shortname_scanner.jar 0 5 http://10.129.204.231/`
`egrep -r ^transf /usr/share/wordlists/* | sed 's/^[^:]*://' > /tmp/list.txt`

|**Part**|**Description**|
|---|---|
|`egrep -r ^transf`|The `egrep` command is used to search for lines containing a specific pattern in the input files. The `-r` flag indicates a recursive search through directories. The `^transf` pattern matches any line that starts with "transf". The output of this command will be lines that begin with "transf" along with their source file names.|
|`\|`|The pipe symbol (`\|`) is used to pass the output of the first command (`egrep`) to the second command (`sed`). In this case, the lines starting with "transf" and their file names will be the input for the `sed` command.|
|`sed 's/^[^:]*://'`|The `sed` command is used to perform a find-and-replace operation on its input (in this case, the output of `egrep`). The `'s/^[^:]*://'` expression tells `sed` to find any sequence of characters at the beginning of a line (`^`) up to the first colon (`:`), and replace them with nothing (effectively removing the matched text). The result will be the lines starting with "transf" but without the file names and colons.|
|`> /tmp/list.txt`|The greater-than symbol (`>`) is used to redirect the output of the entire command (i.e., the modified lines) to a new file named `/tmp/list.txt`.|

`gobuster dir -u http://10.129.204.231/ -w /tmp/list.txt -x .aspx,.asp`
