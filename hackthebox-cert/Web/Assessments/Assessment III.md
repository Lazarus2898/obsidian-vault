# Questions
1.  What is the hardcoded password for the database connection in the MultimasterAPI.dll file?

# The Given
```bash
RDP to with user "Administrator" and password "xcyj8izxNVzhf4z"
```
During our penetration test our team found a Windows host running on the network and the corresponding credentials for the Administrator. It is required that we connect to the host and find the `hardcoded password` for the MSSQL service.
# Walkthrough
RDPing into it, and doing a mass search `Get-ChildItem -Recurse -Path C:\ -Include "MultimasterAPI.dll" -File -ErrorAction SilentlyContinue`
`C:\inetpub\wwwroot\bin`
Grabbing the MultimasterAPI.dll and tossing it into `DnSpy`.
Finally, going through the files, was able to find the hardcoded credentials for the question.
`/mulimasterapi.controllers/colleaguecontroller/getColleagues`