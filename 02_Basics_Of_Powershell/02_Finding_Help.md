<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Getting help
Gaining confidence using the built-in help system is an important part of working with PowerShell. In PowerShell, help is extensive; authors can easily write their own help content when working with scripts and script modules.

A number of commands are available to interact with the help system, as follows:
```
Get-Help
Save-Help
Update-Help
```

Before exploring these commands, the concept of updatable help should be discussed.

**Updatable Help**
Updatable help was introduced with PowerShell 3. It gives authors the option to store the most recent versions of their help documentation outside of PowerShell on web servers.

Which modules support updatable help?A list of modules that support updatable help may be viewed by running the following command: Get-Module -ListAvailable | Where-Object HelpInfoURI -like *
Help for the core components of PowerShell is no longer a part of the Windows Management Framework package and must be downloaded before it can be viewed. The first time Get-Help is run, you will be prompted to update help.

If the previous prompt is accepted, PowerShell will attempt to download content for any module that supports updatable help.

Computers with no internet access or computers behind a restrictive proxy server may not be able to download the help content. If PowerShell is unable to download help, it can only show a small amount of discoverable information about a command; for example, without downloading help, the content for the Out-Null command is minimal, as shown in the following code:

```powershell
PS C:\windows\system32> Get-Help Out-Null

NAME
    Out-Null
    
SYNTAX
    Out-Null [-InputObject <psobject>] [<CommonParameters>]
    

ALIASES
    None
    

REMARKS
    Get-Help cannot find the Help files for this cmdlet on this computer. 
    It is displaying only partial help.
        -- To download and install Help files for the module that 
           includes this cmdlet, use Update-Help.
        -- To view the Help topic for this cmdlet online, type: 
           "Get-Help Out-Null -Online" or go to
           http://go.microsoft.com/fwlink/?LinkID=113366. 
 ```
 Updatable helps as a help file that may be viewed using the following command:
```powershell
Get-Help about_Updatable_Help 
```
**The Get-Help command**
Without any arguments or parameters, Get-Help will show introductory help about the help system. This content is taken from the default help file (Get-Help default); a snippet of this is as follows:
```powershell
PS> Get-Help

TOPIC
    Windows PowerShell Help System

SHORT DESCRIPTION
    Displays help about Windows PowerShell cmdlets and concepts. 

LONG DESCRIPTION
    Windows PowerShell Help describes Windows PowerShell cmdlets,
```

The help content can be long:
The help content, in most cases, will not fit on a single screen. The help command differs from Get-Help in that it pauses (waiting for a key to be pressed) after each page. Let's look at an example:
help default
The previous command is equivalent to running Get-Help and piping it into the more command:
```powershell
Get-Help default | more
```
Alternatively, Get-Help can be asked to show a window:
```powershell
Get-Help default -ShowWindow
```
The available help content may be listed using either of the following two commands:
```powershell
Get-Help * 
Get-Help -Category All
```
Help for a command may be viewed as follows:
```powershell
Get-Help <CommandName>
```
Let's look at an example:
```powershell
Get-Help Get-Variable 
```
The help content is broken down into a number of visible sections: name, synopsis, syntax, description, related links, and remarks.

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/02_Basics_Of_Powershell/03_Perf_labs.md" > Continue to Performance Labs </a>
