# Basic commands

```Powershell
# Help pages for a command
Get-Help <command>

# Shows IP Configuration
Get-NetIPAddress

# Get pwd
Get-Location

# Finds modules that are loaded into the session
Get-Module

# Read a file
Get-Content

# Make a file
New-Item
```

# User Group Management

```Powershell
# Displaying Local users on the host
Get-LocalUser

# Domain Users
Get-ADUser

# Shows services of a host
Get-Service

# Starting a service
Start-Service <service>

# Execute Command
Invoke-Command

# Enables WinRM on a host
winrm quickconfig
```

# Filtering

```Powershell
# Shows properties/methods of a file
Get-Member

Looks into sub directories for a file
Get-ChildItem -Recursive
```
