<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

**Dissecting the Powershell Command** 

Figure Below shows the basic anatomy of a complex PowerShell command. We call this the full-form syntax of a command. 
We’re showing a somewhat complex command so you can see all of the things that might show up.
![image](https://user-images.githubusercontent.com/47218880/61657966-e7257700-ac89-11e9-92df-afed7148dc2c.png)

To make sure you’re completely familiar with PowerShell’s rules, 
let’s cover each of the elements in the previous figure in more detail:

- The cmdlet name is Get-EventLog. PowerShell cmdlets always have this verb-noun naming format. We explain more about cmdlets in the next section.
- The first parameter name is -LogName, and it’s being given the value Security. Because the value doesn’t contain any spaces or punctuation, it doesn’t need to be in quotation marks.
- The second parameter name is -ComputerName, and it’s being given two values: WIN8 and SERVER1. These are in a comma-separated list, and because neither value contains spaces or punctuation, neither value needs to be inside quotation marks.
- The final parameter, -Verbose, is a switch parameter. That means it doesn’t get a value; specifying the parameter is sufficient.
- Note that there’s a mandatory space between the command name and the first parameter.
- Parameter names always start with a dash (-).
- There’s a mandatory space after the parameter name, and between the parameter’s value and the next parameter name.
- There’s no space between the dash (-) that precedes a parameter name and the parameter name itself.
- Nothing here is case-sensitive.

```
Get used to these rules. Start being sensitive about accurate, neat typing. 
Paying attention to spaces and dashes and other rules will minimize 
the silly errors that PowerShell throws at you.

```
 
**The cmdlet naming convention**

First, let’s discuss some terminology. As far as we know, we’re the only ones who use this terminology in everyday conversation, but we do it consistently, so we may as well explain:

- A **cmdlet** is a native PowerShell command-line utility. These exist only inside PowerShell and are written in a .NET Framework language such as C#. The word cmdlet is unique to PowerShell, so if you add it to your search keywords on Google or Bing, the results you get back will be mainly PowerShell-related. The word is pronounced **command-let**.
- A **function** can be similar to a cmdlet, but rather than being written in a .NET language, functions are written in PowerShell’s own scripting language.
- A **workflow** is a special kind of function that ties into PowerShell’s workflow execution system.
- An **application** is any kind of external executable, including command-line utilities such as Ping and Ipconfig.
- **Command** is the generic term that we use to refer to any or all of the preceding terms.

Microsoft has established a naming convention for cmdlets. That same naming convention **should** be used for functions and workflows, too, although Microsoft can’t force anyone but its own employees to follow that rule.

The rule is this: Names start with a standard verb, such as **Get** or **Set** or New or Pause. You can run **Get-Verb** to see a list of allowable verbs (you’ll see about 100, although only about a dozen are common). After the verb is a dash, followed by a singular noun, such as Service or Process or EventLog. Developers get to make up their own nouns, so there’s no **Get-Noun** cmdlet to display them all.

What’s the big deal about this rule? Well, suppose we told you that there were cmdlets named *New-Service, Get-Service, Get-Process, Set-Service*, and so forth. Could you guess what command would create a new Exchange mailbox? Could you guess what command would modify an Active Directory user? If you guessed *New-Mailbox*, you got the first one right. If you guessed Set-User, you were close: it’s *Set-ADUser*, and you’ll find the command on domain controllers in the Active-Directory module. The point is that by having this consistent naming convention with a limited set of verbs, it becomes possible for you to guess at command names, and you could then use *Help* or *Get-Command*, along with wildcards, to validate your guess. It becomes easier for you to figure out the names of the commands you need, without having to run to Google or Bing every time.

```
OK,OK
```
Not all of the so-called verbs are really verbs. Although Microsoft officially uses the term verb-noun naming convention, you’ll see “verbs” like *New, Where*, and so forth. You’ll get used to it.

**Powershell Example Code**

We’ve said PowerShell is for solving problems that involve writing code. By now you’re probably asking “Dude! Where’s my code?” Enough talk, let’s see some example code! First, we’ll revisit the Get-ChildItem example. This time, instead of displaying the directory listing, you’ll save it into a file using output redirection like in other shell environments. In the following example, you’ll use Get-ChildItem to get information about a file named somefile.txt in the root of the C: drive. Using redirection, you’ll direct the output into a new file, c:\foo.txt, and then use the type command to display what was saved. Here’s what this looks like:


```powershell
PS> Get-ChildItem -Path C:\somefile.txt

    Directory: C:\

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       29/05/2017     13:58          25424 somefile.txt
```
```
PowerShell has aliases for many cmdlets so dir C:\somefile.txt and ls C:\somefile.txt would both work. 
It is best practice to reserve aliases for interactive usage and not use them in scripts.
We’ll usually use the full cmdlet name but may occasionally use aliases to save space.
```
Next, instead of displaying the directory listing, you’ll save it into a file using output redirection as in other shell environments. In the following example, you’ll get information about a file named somefile.txt in the root of the C: drive. Using redirection, you direct the output into a new file, c:\foo.txt, and then use the Get-Content (you can use the alias of cat or type if you prefer) command to display what was saved. Here’s what this looks like:

```powershell
PS> Get-ChildItem -Path C:\somefile.txt > c:\foo.txt
PS> Get-Content -Path C:\foo.txt

    Directory: C:\

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       29/05/2017     13:58          25424 somefile.txt
```
As you can see, commands work more or less as you’d expect. Let’s go over other things that should be familiar to you.

```
On your system choose any file that exists and the example will work fine, 
though obviously, the output will be different.
```

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/03_Powershell_Commands/02_Aliases.md" > Continue to Next Topic </a>
