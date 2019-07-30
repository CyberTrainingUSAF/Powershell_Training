<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## COMPARISONS

Almost all of the scripting bits we’ll introduce in this chapter rely on comparisons. That is, you give them some statement that must evaluate to either True or False, and the scripting constructs base their behavior on that result. In order to make a comparison in PowerShell, you use a comparison operator. PowerShell’s core ones are as follows:
```
-eq—Equal to
-ne—Not equal to
-gt—Greater than
-ge—Greater than or equal to
-lt—Less than
-le—Less than or equal to
```
For string comparisons, these are case-insensitive by default, which means “Hello” and “HELLO” are the same. If you explicitly need a case-sensitive comparison, add a c to the front of the operator name, as in –ceq or –cne.

When you use these operators, PowerShell will return a True/False value:
```powershell
PS C:\> 1 -eq 1
True
PS C:\> 5 -gt 10
False
PS C:\> 'don' -eq 'jeff'
False
PS C:\> 'don' -eq 'Don'
True
PS C:\> 'don' -ceq 'Don'
False
```
PowerShell doesn’t have the same extensive range of operators as some languages. For example, there’s no “exactly equal to” comparison that forbids the shell’s parser from coercing a data type into another type.

---
<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/02_Basics_Of_Powershell/07_Wildcards.md" > Contiue to Next Topic </a>
