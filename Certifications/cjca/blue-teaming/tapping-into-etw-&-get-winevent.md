# Question 1
Replicate executing Seatbelt and SilkETW as described in this section and provide the ManagedInteropMethodName that starts with "G" and ends with "ion" as your answer. "c:\Tools\SilkETW_SilkService_v8\v8" and "C:\Tools\GhostPack Compiled Binaries" on the spawned target contain everything you need.

1. cd "c:\Tools\SilkETW_SilkService_v8\v8\SilkETW"
2. SilkETW.exe -t user -pn Microsoft-Windows-DotNETRuntime -uk 0x2038 -ot file -p C:\windows\temp\etw.json
3. New Window
4. cd "C:\Tools\GhostPack Compiled Binaries"
5. Seatbelt.exe TokenPrivileges
6. Select-String -Path C:\windows\temp\etw.json -Pattern '"ManagedInteropMethodName":"G[^"]*ion"'
7. "GetTokenInformation"

# Question 2
Utilize the Get-WinEvent cmdlet to traverse all event logs located within the "C:\Tools\chainsaw\EVTX-ATTACK-SAMPLES\Lateral Movement" directory and determine when the \\*\PRINT share was added. Enter the time of the identified event in the format HH:MM:SS as your answer.

1. Get-WinEvent -Path (Get-ChildItem "C:\Tools\chainsaw\EVTX-ATTACK-SAMPLES\Lateral Movement\*.evtx").FullName -ErrorAction SilentlyContinue | Where-Object { $_.Id -eq 5142 -and $_.Message -match "PRINT" } | Select-Object TimeCreated, Id, Message | Format-List
