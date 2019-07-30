<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

## The Red Text

It’s inevitable that you’ll see some ugly red text as you start working with PowerShell—and probably from time to time even after you’re an expert-level shell user. Happens to us all. But don’t let the red text stress you out. (Personally, it takes us back to high school English class and poorly written essays, so stress is putting it mildly.)

The alarming red text aside, PowerShell’s error messages are intended to be helpful. For example, as shown in the figure below, they try to show you exactly where PowerShell ran into trouble.

![image](https://user-images.githubusercontent.com/47218880/61723915-89993500-ad32-11e9-99f8-2718f342fcbb.png)

Error messages almost always include the line and char (character) number where PowerShell got confused. In the figure above, it’s line 1, char 1—right at the beginning. It’s saying, “You typed get, and I have no idea what that means.” That’s because we typed the command name wrong: It’s supposed to be Get-Command, not Get Command. Oops. What about the next figure?

![image](https://user-images.githubusercontent.com/47218880/61724057-ccf3a380-ad32-11e9-8784-78add2912391.png)

The error message in figure above , “Second path fragment must not be a drive or UNC name,” is confusing. What second path? We didn’t type a second path. We typed one path, c:\windows, and a command-line parameter, /s. Right?

Well, no. One of the easiest ways to solve this kind of problem is to read the help and to type the command out completely. If we’d typed Get-ChildItem -path C:\Windows, we’d have realized that /s isn’t the correct syntax. We meant -recurse. Sometimes the error message might not seem helpful—and if it seems like you and PowerShell are speaking different languages, you are. PowerShell obviously isn’t going to change its language, so you’re probably the one in the wrong, and consulting the help and -spelling out the entire command, parameters and all, is often the quickest way to solve the problem. And don’t forget to use Show-Command to figure out the right syntax.

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/03_Powershell_Commands/05_Confusion.md" > Continue to Next Topic </a>
