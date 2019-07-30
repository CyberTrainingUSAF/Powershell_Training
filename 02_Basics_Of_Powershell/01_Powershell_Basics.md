<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Powershell or CMD

Is Powershell just the command line prompt with a blue background

**Powershell and cmd.exe similarities**
  - Both provide a command-line environment
  - Both allow the creation of scripts
  -The old 'trustees" in the Cmd.exe (commands) still work in Powershell
    -Dir
    -Cls
    -Etc.
    
**So What's the difference between Powershell and Cmd.exe?**

  - Cmd.exe provided a command-line interface for a very limited set of functionality
  - Cmd.exe is a text-based scripting environment
    -Makes dealing with command results difficult
  - Cmd.exe does not provide access to amny aspects of the OS and Applications
  - Powershell is an object-based scripting language
    -Everythin is an object
    -Makes dealing with command results very easy and efficient
  - Powershell provides access to the .NET Framework, the building blocks of Windows functionalities.

**Powershell commands**

Starting at the beginning, here’s the traditional “Hello world” program in PowerShell:

```powershell
'hello world'
```
But “Hello world” itself isn’t interesting. Here’s something a bit more complicated:
```powershell
Get-ChildItem -Path $env:windir\*.log |
Select-String -List error |
Format-Table Path,LineNumber –AutoSize
```
Although this is more complex, you can probably still figure out what it does. It searches all the log files in the Windows directory, looking for the string “error”, and then prints the full name of the matching file and the matching line number. “Useful, but not special,” you might think, because you can easily do this using cmd.exe on Windows or bash on UNIX. What about the “big, really big” thing? Well, how about this example:
```powershell
([xml] [System.Net.WebClient]::new().
    DownloadString('http://blogs.msdn.com/powershell/rss.aspx')).
        RSS.Channel.Item |
            Format-Table title,link
```
Now we’re getting somewhere. This script downloads the RSS feed from the PowerShell team blog and then displays the title and a link for each blog entry. By the way, you weren’t expected to figure out this example yet. If you did, you can move to the head of the class!

One last example:
```powershell
using assembly  System.Windows.Forms
using namespace System.Windows.Forms
$form = [Form] @{
    Text = 'My First Form'
}
$button = [Button] @{
    Text = 'Push Me!'
    Dock = 'Fill'
}
$button.add_Click{
    $form.Close()
}
$form.Controls.Add($button)
$form.ShowDialog()
```
This script uses the Windows Forms library (WinForms) to build a GUI that has a single button displaying the text “Push Me!” Figure below shows the window this script creates.

![image](https://user-images.githubusercontent.com/47218880/61659686-ae879c80-ac8d-11e9-96bf-28fca1193501.png)

When you click the button, it closes the form and exits the script. With this you go from "Hello world" to a GUI application in less than two pages.

Let’s come back down to Earth for a minute. The intent of this section is to set the stage for understanding PowerShell—what it is, what it isn’t, and, almost as important, why the PowerShell team made the decisions they made in designing the PowerShell language. First, a philosophical digression: while under development, from 2002 until the first public release in 2006, the codename for this project was Monad. The name Monad comes from The Monadology by Gottfried Wilhelm Leibniz, one of the inventors of calculus. Here’s how Leibniz defined the Monad:

The Monad, of which we shall here speak, is nothing but a simple substance, which enters into compounds. By “simple” is meant “without parts.”

Gottfried Wilhelm Leibniz, The Monadology (translated by Robert Latta)

In The Monadology, Leibniz describes a world of irreducible components from which all things could be composed. This captures the spirit of the project: to create a toolkit of simple pieces you compose to create complex solutions.

**What is Powershell?** 

What is PowerShell, and what can you do with it? Ask a group of PowerShell users and you’ll get different answers:
```
PowerShell is a command-line shell.
PowerShell is a scripting environment.
PowerShell is an automation engine.
```
These are all part of the answer. We prefer to say PowerShell is a tool you can use to manage your Microsoft-based machines and applications that programs consistency into your management process. The tool is attractive to administrators and developers in that it can span the range of command line, simple and advanced scripts, to real programs.

> Typical Command Example:
![image](https://user-images.githubusercontent.com/47218880/61650708-e258c700-ac79-11e9-9ff8-8521ec686382.png)

> Powershell Command Example:
![image](https://user-images.githubusercontent.com/47218880/61650874-52ffe380-ac7a-11e9-8d52-9f9f5c7274c3.png)

![image](https://user-images.githubusercontent.com/47218880/61651077-b9850180-ac7a-11e9-9148-fe7500f712fc.png)

**So What's the difference between Powershell and Cmd.exe?**
  * Powershell provides the programming power and accessibility of an object oriented programming language in a scripting environment.
  
---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/01_Intro_to_Powershell/02_Perf_labs.md" > Continue to Performance Labs </a>
