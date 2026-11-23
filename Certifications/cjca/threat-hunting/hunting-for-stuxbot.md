# The Walkthrough
### Question 1
Navigate to http://[Target IP]:5601 and follow along as we hunt for Stuxbot. In the part where default.exe is under investigation, a VBS file is mentioned. Enter its full name as your answer, including the extension.

1. Go to discover and change the dataview to `windows*`
2. Set the range to the last `15 years`
3. Do a KQL query of `process.name:"default.exe" AND file.extension:"vbs"`
4. Pulling down the arrow on the right of the alert will provide the necessary information.

### Question 2
Stuxbot uploaded and executed mimikatz. Provide the process arguments (what is after .\mimikatz.exe, ...) as your answer.
1. Changing the KQL query to `process.name:"mimikatz.exe"` or `process.command_line:*mimikatz.exe*`
2. You will have 1-2 alerts.
3. Going down the process.args, `lsadump::dcsync /domain:eagle.local /all /csv, exit`

### Question 3
Some PowerShell code has been loaded into memory that scans/targets network shares. Leverage the available PowerShell logs to identify from which popular hacking tool this code derives. Answer format (one word): P____V___
1. Changing the KQL query to `message:*Share* AND event.code:4104`
2. This will provide hints of a tool called Powerview.


# Skill Assessment
Hunt 1: Create a KQL query to hunt for "Lateral Tool Transfer" to C:\Users\Public. Enter the content of the user.name field in the document that is related to a transferred tool that starts with "r" as your answer.
1. `file.path:*Users*Public*`
2. Then looking at the tools used `Rubeus.exe` is mentioned
3. `svc-sql`

Hunt 2: Create a KQL query to hunt for "Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder". Enter the content of the registry.value field in the document that is related to the first registry-based persistence action as your answer.
1. `event.code:13 AND message:"*Run*"`
2. Then looking through the logs finding a suspicious registry key value
3. `LgvHsviAUVTsIN`

Hunt 3: Create a KQL query to hunt for "PowerShell Remoting for Lateral Movement". Enter the content of the winlog.user.name field in the document that is related to PowerShell remoting-based lateral movement towards DC1.

