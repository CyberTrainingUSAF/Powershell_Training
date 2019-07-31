<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Modules

**What is a module?**
Modules were introduced with the release of PowerShell version 2.0. Modules represented a significant step forward over snap-ins. Unlike snap-ins, modules do not have to be formally installed or registered for use with PowerShell.

It is most common to find a module that targets a specific system or focuses on a small set of related operations. For example, the Microsoft.PowerShell.LocalAccounts module contains commands for interacting with the local account database (users and groups).

**A module may be binary, script, dynamic, or manifest:**

* **Binary module:** This is written in a language, such as C# or VB.NET, and then compiled into a library (DLL)
* **Script module:** This is a collection of functions written in the PowerShell language. The commands typically reside in a script module file (PSM1)
* **Dynamic module:** This does not have files associated with it. This is created using the New-Module command. 

The following command creates a very simple dynamic module that adds the Get-Number command:

```powershell
New-Module -Name TestModule -ScriptBlock { 
    function Get-Number { return 1 } 
} 
```

A manifest module combines different items to make a single consistent module. For example, a manifest may be used to create a single module out of a DLL containing cmdlets and a script containing functions. For example, Microsoft.PowerShell.Utility is a manifest module that combines a binary and script module.

A manifest module may also be used to build commands based on WMI classes. The cmdlets-over-objects feature was added with PowerShell 3, an XML file with a cdxml extension (Cmdlet definition XML). For example, the Defender module creates commands based on WMI classes in ROOT/Microsoft/Windows/Defender namespace.

The manifest module file serves a number of purposes, as follows:

* Describing the files that should be loaded (such as a script module file, a binary library, and a cmdlet definition XML file)
* Listing any dependencies the module may have (such as other modules, .NET libraries, or other DLL files)
* Listing the commands that should be exported (made available to the end user) Recording information about the author or the project

When loading a module with a manifest, PowerShell will try and load any listed dependencies. If a module fails to load because of a dependency, the commands written as part of the module will not be imported.

**MODULE BASICS**

In this section, we’ll cover the basic information needed to use PowerShell modules. The first thing to know is that the module features in PowerShell are exposed through cmdlets, not language keywords. For example, you can get a list of the module commands using the Get-Command command:

```powershell
PS> Get-Command -Noun Module* | Format-Wide -Column 3

Find-Module              Install-Module           Publish-Module
Save-Module              Uninstall-Module         Update-Module
Update-ModuleManifest    Export-ModuleMember      Get-Module
Import-Module            New-Module               New-ModuleManifest
Remove-Module            Test-ModuleManifest
```

**Note** that in the command name pattern, you use wildcards because there are a couple of different types of module cmdlets. These cmdlets and their descriptions are shown

![image](https://user-images.githubusercontent.com/47218880/61807494-43f66e00-adff-11e9-9eba-54e22c8f39f9.png)

**Finding modules on the system**
The Get-Module cmdlet is used to find modules—either the modules that are currently loaded or the modules that are available to load. The signature for this cmdlet is shown

![image](https://user-images.githubusercontent.com/47218880/61807569-68eae100-adff-11e9-8e1e-274021f95e5a.png)

When run with no options, Get-Module lists all the top-level modules loaded in the current session. If -All is specified, both explicitly loaded and nested modules are shown. (We’ll explain the difference between top-level and nested in a minute.) If -ListAvailable is specified, Get-Module lists all the modules available to be loaded based on the current $ENV:PSModulePath setting. If both –ListAvailable and -All are specified, the contents of the module directories are shown, including subdirectories.

Let’s try this and see how it works; running Get-Module with no parameters on a new PowerShell v5 session shows three modules installed by default:

```
Microsoft.PowerShell.Management
Microsoft.PowerShell.Utility
PSReadline
```
Let’s see what’s available for loading on the system. You can use the -ListAvailable parameter on Get-Module to find the system modules that are available. (In this example, the output is filtered using the Where-Object cmdlet so you don’t pick up any non-system modules.)
```powershell
PS> Get-Module -ListAvailable | where { $_.path -match "System32" }
```
And you see 74 modules listed on a new Windows 10 machine (PowerShell v5.1)

---
<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/04_Powershell_Scripts/09_Challenge_Labs.md" > Continue to Performance Labs </a>

