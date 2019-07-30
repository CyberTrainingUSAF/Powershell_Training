<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---
## Collections - Performance Labs

PowerShell’s –contains and –in operators (-in was introduced in v3, so don’t look for it in v2) operate against collections of objects. They get a little tricky, and people almost always confuse them with wildcard operators. For example, we see this a lot:

```powershell
If ("DC" –in $servername) {
  $IsDomainController = $True
}
```
This doesn’t work the way you might think. It reads just fine in English, but it’s not what the operator does. If you start with an array, you can use these operators to determine whether the array (or collection) contains a particular object:
```powershell
$array = @("one","two","three")
$array –contains "one"
$array –contains "five"
"two" –in $array
"bob" –in $array
```
> Try it Now
Go ahead and run those five lines of code in PowerShell, typing the lines one at a time and pressing Enter after each.

## Troubleshooting comparisons
About 4 times out of 10, we find that script bugs are due to a comparison that isn’t working the way you expect. Our best advice for troubleshooting these is to stop working on your script, jump into the PowerShell console, and try the comparison there. For example, what will this produce?
```powershell
"55" –eq 55
```
> Try it Now
We’re not going to give you the answer—try it, and see if you can explain to yourself why it did what it did.

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/03_Powershell_Commands/01_Anatomy_of_a_command.md" > Continue to Next Topic </a>
