Thick client applications are the applications that are installed locally on our computers. Unlike thin client applications that run on a remote server and can be accessed through the web browser, these applications do not require internet access to run, and they perform better in processing power, memory, and storage capacity. Thick client applications are usually applications used in enterprise environments created to serve specific purposes. Such applications include project management systems, customer relationship management systems, inventory management tools, and other productivity software. These applications are usually developed using Java, C++, .NET, or Microsoft Silverlight.

In a `.NET` environment, a `thick client`, also known as a `rich` client or `fat` client, refers to an application that performs a significant amount of processing on the client side rather than relying solely on the server for all processing tasks. As a result, thick clients can provide a better performance, more features, and improved user experiences compared to their `thin client` counterparts, which rely heavily on the server for processing and data storage.

# Tools
| Tools for Information gathering                 |                                                             |                                                                                     |                                                                             |
| ----------------------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| [CFF Explorer](https://ntcore.com/?page_id=388) | [Detect It Easy](https://github.com/horsicq/Detect-It-Easy) | [Process Monitor](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon) | [Strings](https://learn.microsoft.com/en-us/sysinternals/downloads/strings) |

| Tools for Client Side Attacks           |                                      |                                        |                                                |
| --------------------------------------- | ------------------------------------ | -------------------------------------- | ---------------------------------------------- |
| [Ghidra](https://www.ghidra-sre.org/)   | [IDA](https://hex-rays.com/ida-pro/) | [OllyDbg](http://www.ollydbg.de/)      | [Radare2](https://www.radare.org/r/index.html) |
| [dnSpy](https://github.com/dnSpy/dnSpy) | [x64dbg](https://x64dbg.com/)        | [JADX](https://github.com/skylot/jadx) | [Frida](https://frida.re/)                     |

| Tools for Network Side Attacks                  |                                                             |                                                                                     |                                                                             |
| ----------------------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| [CFF Explorer](https://ntcore.com/?page_id=388) | [Detect It Easy](https://github.com/horsicq/Detect-It-Easy) | [Process Monitor](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon) | [Strings](https://learn.microsoft.com/en-us/sysinternals/downloads/strings) |

## Retrieving hardcoded Credentials from Thick-Client Applications
Downloading the tool `ProcMon64` from [SysInternals](https://learn.microsoft.com/en-gb/sysinternals/downloads/procmon) and monitoring the process reveals that the executable indeed creates a temp file in `C:\Users\Matt\AppData\Local\Temp`.

In order to capture the files, it is required to change the permissions of the `Temp` folder to disallow file deletions. To do this, we right-click the folder `C:\Users\Matt\AppData\Local\Temp` and under `Properties` -> `Security` -> `Advanced` -> `cybervaca` -> `Disable inheritance` -> `Convert inherited permissions into explicit permissions on this object` -> `Edit` -> `Show advanced permissions`, we deselect the `Delete subfolders and files`, and `Delete` checkboxes. Finally, we click `OK` -> `Apply` -> `OK` -> `OK` on the open windows. Once the folder permissions have been applied we simply run again the `Restart-OracleService.exe` and check the `temp` folder. The file `6F39.bat` is created under the `C:\Users\cybervaca\AppData\Local\Temp\2`. The names of the generated files are random every time the service is running.
```powershell
# delete the botttom lines
del c:\programdata\monta.ps1
del c:\programdata\oracle.txt
c:\programdata\restart-service.exe
del c:\programdata\restart-service.exe

```
Running `monta.ps1` then `x64debugger`
