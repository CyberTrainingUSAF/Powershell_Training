<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Nicknames for Commands
Although PowerShell command names can be nice and consistent, they can also be long. A command name such as *Set-WinDefaultInputMethodOverride* is a lot to type, even with Tab completion. Although the command name is clear—looking at it, you can probably guess what it does—it’s an awful lot to type.

That’s where PowerShell aliases come in. An alias is nothing more than a nickname for a command. Tired of typing *Get-Service*? Try this:

```powershell
PS C:\> get-alias -Definition "Get-Service"

Capability      Name
----------      ----
Cmdlet          gsv -> Get-Service
```

Now you know that gsv is an alias for **Get-Service**.

When using an alias, the command works in exactly the same way. Parameters are the same, everything is the same—the command name is just shorter. If you’re used to UNIX or Linux, where an alias can also include some parameters, just know that Power-Shell doesn’t work that way.

If you’re staring at an alias (folks on the internet tend to use them as if we’ve all memorized the hundreds of built-in aliases) and can’t figure out what it is, ask help:

```powershell
PS C:\> help gsv

NAME
    Get-Service
SYNOPSIS
    Gets the services on a local or remote computer.
SYNTAX
    Get-Service [[-Name] <String[]>] [-ComputerName <String[]>]
    [-DependentServices [<SwitchParameter>]] [-Exclude <String[]>]
    [-Include <String[]>] [-RequiredServices [<SwitchParameter>]]
    [<CommonParameters>]
    Get-Service [-ComputerName <String[]>] [-DependentServices
    [<SwitchParameter>]] [-Exclude <String[]>] [-Include <String[]>]
    [-RequiredServices [<SwitchParameter>]] -DisplayName <String[]>
    [<CommonParameters>]
    Get-Service [-ComputerName <String[]>] [-DependentServices
    [<SwitchParameter>]] [-Exclude <String[]>] [-Include <String[]>]
    [-InputObject <ServiceController[]>] [-RequiredServices
    [<SwitchParameter>]] [<CommonParameters>]
  ```
    
  When asked for help about an alias, the help system will always display the help for the full command, which includes the command’s complete name.
   
**Above and beyond**

You can create your own aliases by using *New-Alias*, export a list of aliases by using *Export-Alias*, or even import a list of previously created aliases by using *Import-Alias*. When you create an alias, it lasts only as long as your current shell session. Once you close the window, it’s gone. That’s why you might want to export them, so that you can more easily reimport them.

We tend to avoid creating and using custom aliases, though, because they’re not available to anyone but us. If someone can’t look up what *xtd* does, we’re creating confusion and incompatibility.

And *xtd* doesn’t do anything. It’s a fake alias we made up.

We must point out, because PowerShell is now available on non-Windows operating systems, that its concept of alias is a little different from an alias on, say, Linux. On Linux, an alias can be a kind of shortcut for running a command that includes a bunch of parameters. PowerShell doesn’t behave that way. An alias is only a nickname for the command name, and the alias can’t include any predetermined parameters.
   
---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/03_Powershell_Commands/03_Parameters.md" > Continue to Next Topic </a>
