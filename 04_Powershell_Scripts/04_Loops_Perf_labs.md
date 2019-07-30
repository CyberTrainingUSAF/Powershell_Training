

---

## Loops performance Labs

> Type these programming scripts into files and run them and manipulate them to see what they do.

**Foreach**

ForEach has two different meanings in PowerShell. One is a keyword and the other is an alias for the ForEach-
Object cmdlet. The former is described here.

This example demonstrates printing all items in an array to the console host:
```powershell
$Names = @('Amy', 'Bob', 'Celine', 'David')
ForEach ($Name in $Names)
{
Write-Host "Hi, my name is $Name!"
}
```
This example demonstrates capturing the output of a ForEach loop:
```powershell
$Numbers = ForEach ($Number in 1..20) {
$Number # Alternatively, Write-Output $Number
}
```
Like the last example, this example, instead, demonstrates creating an array prior to storing the loop:
```powershell
$Numbers = @()
ForEach ($Number in 1..20)
{
$Numbers += $Number
}
```
**For**
The for statement runs a statement list zero or more times based on an initial setting, a conditional test, and a repeated statement, most often used with numeric counters.

```powershell
for($i = 0; $i -lt 3; $i++)
{
    Write-Output $i
}
```

> For loops are often nested to repeat actions, such as for rows and columns. For example:
```powershell
for($i = 0; $i -lt 3; $i++)
{
    $line = ''
    for($j = 0; $j -lt 3; $j++)
    {
        $line += $i.ToString() + $j.ToString() + '  '
    }
    Write-Output $line
}
```
**Continue and Break decision Structures**

**Continue**
The continue statement immediately returns script flow to the top of the innermost While, Do, For, or ForEach loop.
```powershell
$processes = Get-Process
foreach($process in $processes)
{
    if($process.PM / 1024 / 1024 -le 100)
    {
        continue
    }
    Write-Output ('Process ' + $process.Name + ' is using more than 100 MB RAM.')
}
```
**Break**
The break statement causes Windows PowerShell to immediately exit the innermost While, Do, For, or ForEach loop or Switch code block.
```powershell
$processes = Get-Process
foreach($process in $processes)
{
    if($process.PM / 1024 / 1024 -gt 100)
    {
        Write-Output ('Process ' + $process.Name + ' is using more than 100 MB RAM.')
        break
    }
}
```
**While/Do-While/Do-Until**

**While**
The while statement runs a statement list zero or more times based on the results of a conditional test.
```powershell
$i = 0
while($i -lt 3)
{
    Write-Output $i
    $i++
}
```
**Do While**
The do while statement runs a statement list one or more times based on the affirmative results of a conditional test.
```powershell
$i = 0
do
{
    Write-Output $i
    $i++
}while($i -lt 3)
```
It is important to note that, unlike while, a do while or do until will always execute at least once.

$i = 0
do
{
    Write-Output $i
    $i--
}while($i -gt 0)

###Do Until
The do until statement runs a statement list one or more times based on the negative results of a conditional test.
```powershell
$i = 0
do
{
    Write-Output $i
    $i++
}until($i -ge 3)

```

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/04_Powershell_Scripts/05_Perf_labs.md" > Continue to more Perfance Labs </a>
