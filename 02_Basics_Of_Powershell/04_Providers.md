<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Providers
Providers in PowerShell present access to data that is not normally easily accessible. There are providers for the filesystem, registry, certificate store, and so on. Each provider arranges data so that it resembles a filesystem.

A longer description of Providers may be seen by viewing the about file:
```powershell
Get-Help about_Providers
```
The list of providers available in the current PowerShell session may be viewed by running Get-PSProvider, as shown in the following example:
```powershell
PS> Get-PSProvider
  
Name           Capabilities                       Drives 
----           ------------                       ------ 
Registry       ShouldProcess, Transactions        {HKLM, HKCU}
Alias          ShouldProcess                      {Alias} 
Environment    ShouldProcess                      {Env} 
FileSystem     Filter, ShouldProcess, Credentials {C, D} 
Function       ShouldProcess                      {Function} 
Variable       ShouldProcess                      {Variable}
Certificate    ShouldProcess                      {Cert}
WSMan          Credentials                        {WSMan}
```
Each of the previous providers has a help file associated with it. These can be accessed using the following code:
```powershell
Get-Help -Name <ProviderName> -Category Provider 
```
For example, the help file for the certificate provider may be viewed by running the following code:
```powershell
Get-Help -Name Certificate -Category Provider 
```
A list of all help files for providers may be seen by running the following code:
```powershell
Get-Help -Category Provider 
```

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/02_Basics_Of_Powershell/05_Providers_Perf_labs.md" > Continue to Performance Labs </a>
































