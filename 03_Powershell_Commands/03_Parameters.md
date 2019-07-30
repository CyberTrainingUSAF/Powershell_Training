<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Parameters

---

Here’s where PowerShell gets tricky. We’d love to tell you that everything we’ve shown you so far is the only way to do things, but we’d be lying. And, unfortunately, you’re going to be out on the internet stealing (er, repurposing) other people’s examples, and you’ll need to know what you’re looking at.  In addition to aliases, which are shorter versions of command names, you can also take shortcuts with parameters. You have three ways to do this, each potentially more confusing than the last.

**Truncating Parameter Names**
PowerShell doesn’t force you to type out entire parameter names. Instead of typing -computerName, for example, you could go with -comp. The rule is that you have to type enough of the name for PowerShell to be able to distinguish it. If there’s a -computer-Name parameter, a -common parameter, and a -composite parameter, you’d have to type at least -compu, -commo, and -compo, because that’s the minimum number of letters necessary to uniquely identify each.

If you must take shortcuts, this isn’t a bad one to take, if you can remember to hit Tab after typing that minimum-length parameter so that PowerShell can finish typing the rest of it for you.

**Using Parameters Name Aliases**
Parameters can also have their own aliases, although they can be terribly difficult to find, as they aren’t displayed in the help files or anyplace else convenient. For example, the Get-EventLog command has a -computerName parameter. To discover its aliases, you run this command:
```powershell
PS C:\> (get-command get-eventlog | select -ExpandProperty parameters)
     .computername.aliases
```
We’ve boldfaced the command and parameter names; replace these with whatever command and parameter you’re curious about. In this case, the output reveals that -Cn is an alias for -ComputerName, so you could run this:

```powershell
PS C:\> Get-EventLog -LogName Security -Cn SERVER2 -Newest 10
```
Tab completion will show you the -Cn alias; if you typed Get-EventLog -C and started pressing Tab, it’d show up. But the help for the command doesn’t display -Cn at all, and Tab completion doesn’t indicate that -Cn and -ComputerName are the same thing.

**Using Positional Parameters**
When you’re looking at a command’s syntax in its help file, you can spot positional parameters easily:

```powershell
SYNTAX
    Get-ChildItem [[-Path] <String[]>] [[-Filter] <String>] [-Exclude

    <String[]>] [-Force [<SwitchParameter>]] [-Include <String[]>] [-Name
    [<SwitchParameter>]] [-Recurse [<SwitchParameter>]] [-UseTransaction
    [<SwitchParameter>]] [<CommonParameters>]
```
Here, both -Path and -Filter are positional, and you know that because the parameter name is contained within square brackets. A clearer explanation is available in the full help (help Get-ChildItem -full, in this case), which looks like this:

```powershell
-Path <String[]>
    Specifies a path to one or more locations. Wildcards are
    permitted. The default location is the current directory (.).
    Required?                    false
    Position?                    1
    Default value                Current directory
    Accept pipeline input?       true (ByValue, ByPropertyName)
    Accept wildcard characters?  True
```
That’s a clear indication that the -Path parameter is in position 1. For positional parameters, you don’t have to type the parameter name; you can provide its value in the correct position. For example

```powershell
PS C:\> Get-ChildItem c:\users

    Directory: C:\users
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----         3/27/2016  11:20 AM            Instructor
d-r--         2/18/2016   2:06 AM            Public
```
That’s the same as this:
```powershell
PS C:\> Get-ChildItem -path c:\users

    Directory: C:\users
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----         3/27/2016  11:20 AM            Instructor
d-r--         2/18/2016   2:06 AM            Public
```
The problem with positional parameters is that you’re taking on the responsibility of remembering what goes where. You must type all positional parameters first, in the correct order, before you can add any named (nonpositional) parameters. If you mix up the parameter order, the command fails. For simple commands such as Dir, which you’ve probably used for years, typing -Path feels weird, and almost nobody does it. But for more-complex commands, which might have three or four positional parameters in a row, it can be tough to remember what goes where.

For example, this is a bit difficult to read and interpret:

```powershell
PS C:\> move file.txt users\instructor\
```
This version, which uses parameter names, is easier to follow:

```powershell
PS C:\> move -Path c:\file.txt -Destination \users\instructor\
```
This version, which puts the parameters in a different order, is allowed when you use the parameter names:
```poweshell
PS C:\> move -Destination \users\instructor\ -Path c:\file.txt
```
We tend to recommend against using positional (unnamed) parameters unless you’re banging out something quick and dirty at the command line. In anything that will persist, such as a batch file or a blog post, include all of the parameter names. We do that as much as possible in this book, except in a few instances where we have to shorten the command line to make it fit within the printed pages.

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/03_Powershell_Commands/04_Errors.md" > Continue to Next Topic </a>


