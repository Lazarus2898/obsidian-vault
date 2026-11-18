# Question 1
Replicate the DLL hijacking attack described in this section and provide the SHA256 hash of the malicious WININET.dll as your answer. "C:\Tools\Sysmon" and "C:\Tools\Reflective DLLInjection" on the spawned target contain everything you need.

1. cd C:\Tools\Sysmon
2. sysmon.exe -i -accepteula -h md5,sha256,imphash -l -n
3. sysmon.exe -c sysmonconfig-export.xml
4. Look for

```xml
<RuleGroup name="" groupRelation="or">
  <ImageLoad onmatch="include">
  </ImageLoad>
</RuleGroup>
```
5. Switch to exclude
6. Apply changes (sysmon.exe -c sysmonconfig-export.xml)
7. cd C:\Tools\"Reflective DLLInjection"
8. mkdir C:\Users\Administrator\Desktop\hijack
9. copy C:\Windows\System32\calc.exe C:\Users\Administrator\Desktop\hijack\
10. copy C:\Tools\"Reflective DLLInjection"\reflective_dll.x64.dll C:\Users\Administrator\Desktop\hijack\WININET.dll
11. cd C:\Users\Administrator\Desktop\hijack
12. calc.exe


# Useful information
### Core Architecture (know this flow for exam diagrams)

Providers → generate events (the source)

Controllers → start/stop trace sessions, enable/disable providers (e.g., logman.exe)

Sessions → the subscription linking providers to consumers

Consumers → receive/process events (often written to .ETL files)

Channels → logical containers that filter/organize events; only providers with a Channel property can be consumed by the Event Log

### Four Provider Types (common multiple-choice trap)

MOF providers — Managed Object Format schemas

WPP providers — “Windows Software Trace Preprocessor,” macro-based, low-level kernel/debugging

Manifest-based providers — XML manifest defines event structure (modern standard)

TraceLogging providers — newer, simplified API, minimal code overhead


### Key Commands (very testable)


logman.exe query -ets → lists all running Event Tracing Sessions system-wide (the -ets flag is required — without it, logman won’t see trace sessions)

logman.exe query "<SessionName>" -ets → shows session details: providers, GUIDs, Level, KeywordsAny

logman.exe query providers → lists all available providers + GUIDs on the system

logman.exe query providers <ProviderName> → shows a specific provider’s keywords, levels, and PIDs currently using it

Pipe to findstr to filter long provider lists (e.g., logman.exe query providers | findstr "Winlogon")



### Useful Providers to Memorize (map provider → detection use case)



Microsoft-Windows-Kernel-Process → process injection/hollowing

Microsoft-Windows-Kernel-File → unauthorized file access, ransomware

Microsoft-Windows-Kernel-Network → C2 traffic, exfiltration

Microsoft-Windows-Kernel-Registry → persistence mechanisms

Microsoft-Windows-PowerShell → suspicious PowerShell/script block logging

Microsoft-Windows-CodeIntegrity → unsigned/malicious driver loading

Microsoft-Windows-DNS-Client → DNS tunneling/C2 via DNS

Microsoft-Windows-TerminalServices-LocalSessionManager → suspicious RDP activity

OpenSSH → SSH brute force/auth attempts

WinRM → lateral movement via remote management



### Restricted Providers (likely exam concept)

Microsoft-Windows-Threat-Intelligence = high-value but restricted

Requires Protected Process Light (PPL) status to access

PPL requires Microsoft certification/signing — only legitimate AV/EDR vendors typically get this

Used heavily in DFIR for detecting sophisticated/evasive attacks






