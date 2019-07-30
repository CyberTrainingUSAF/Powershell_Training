<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Foreach
The foreach loop executes against each element of a collection using the following notation:
```
foreach (<element> in <collection>) { 
    <body-statements> 
} 
```
For example, the foreach loop may be used to iterate through each of the processes returned by Get-Process:
```
foreach ($process in Get-Process) { 
    Write-Host $process.Name 
}  
```
If the collection is $null or empty, the body of the loop will not execute.

## For
The for loop is typically used to step through a collection using the following notation:
```
for (<intial>; <exit condition>; <repeat>){ 
    <body-statements>
 } 
 ```
Initial represents the state of a variable before the first iteration of the loop. This is normally used to initialize a counter for the loop.

The exit condition must be true as long as the loop is executing.

Repeat is executed after each iteration of the body and is often used to increment a counter.

The for loop is most often used to iterate through a collection, for example:
```
$processes = Get-Process 
for ($i = 0; $i -lt $processes.Count; $i++) { 
    Write-Host $processes[$i].Name
 } 
 ```
The for loop provides a significant degree of control over the loop and is useful where the step needs to be something other than simple ascending order. For example, the repeat may be used to execute the body for every third element:
```
for ($i = 0; $i -lt $processes.Count; $i += 3) { 
    Write-Host $processes[$i].Name
 } 
 ```
The loop parameters may also be used to reverse the direction of the loop, for example:
```
for ($i = $processes.Count - 1; $i -ge 0; $i--) { 
    Write-Host $processes[$i].Name
 } 

```
## Do until and do while
do untiland do while each execute the body of the loop at least once as the condition test is at the end of the loop statement. Loops based on do until will exit when the condition evaluates to true; loops based on do while will exit when the condition evaluates to false.

Do loops are written using the following notation:
```
do { 
    <body-statements> 
} <until | while> (<condition>) 
```
do until is suited to exit conditions which are expected to be positive. For example, a script might wait for a computer to respond to ping:
```
do { 
    Write-Host "Waiting for boot" 
    Start-Sleep -Seconds 5 
} until (Test-Connection 'SomeComputer' -Quiet -Count 1) 
```
The do while loop is more suitable for exit conditions which are negative. For example, a loop might wait for a remote computer to stop responding to ping:
```
do { 
    Write-Host "Waiting for shutdown" 
    Start-Sleep -Seconds 5 
} while (Test-Connection 'SomeComputer' -Quiet -Count 1) 
```
## While
As the condition for a while loop comes first, the body of the loop will only execute if the condition evaluates to true:
```
while (<condition>) { 
    <body-statements> 
} 
```
For example, a while loop may be used to wait for something to happen. For example, it might be used to wait for a path to exist:
```
while (-not (Test-Path $env:TEMP\test.txt -PathType Leaf)) { 
    Start-Sleep -Seconds 10 
} 
```
## Break and continue
Break can be used to end a loop early. The loop in the following example would continue to 20; break is used to stop the loop at 10:
```
for ($i = 0; $i -lt 20; $i += 2) {
     Write-Host $i 
    if ($i -eq 10) {
         break    # Stop this loop 
    } 
} 
```
Break acts on the loop it is nested inside. In the following example, the inner loop breaks early when the variable i is less than or equal to 2:
```
PS> $i = 1 # Initial state for i
while ($i -le 3) {
Write-Host "i: $i"
$k = 1 # Reset k
while ($k -lt 5) {
Write-Host " k: $k"
$k++ # Increment k
if ($i -le 2 -and $k -ge 3) {
break
}
}
$i++ # Increment i
}
i: 1
k: 1
k: 2
i: 2
k: 1
k: 2
i: 3
k: 1
k: 2
k: 3
k: 4
```
The continue keyword may be used to move on to the next iteration of a loop immediately. For example, the following loop executes a subset of the loop body when the value of the variable i is less than 2:
```
for ($i = 0; $i -le 5; $i++) { 
    Write-Host $i 
    if ($i -lt 2) { 
        continue    # Continue to the next iteration 
    } 
    Write-Host "Remainder when $i is divided by 2 is $($i % 2)" 
} 

```

In the DemoWhileLessThan.ps1 script, you use the expanding string to print the status message of the value of the $i variable during each trip through the While loop. You suppress the expanding nature of the expanding string for the first $i variable so you can tell which variable you are talking about. As soon as you have done this, you increment the value of the $i variable by one. To do this, you use the $i++ syntax. This is identical to saying the following.
```
$i = $i + 1
```
The advantage is that the $i++ syntax requires less typing. The DemoWhileLessThan.ps1 script is shown here.

> DemoWhileLessThan.ps1

```powershell
$i = 0
While ($i -lt 5)
 {
  "`$i equals $i. This is less than  5"
  $i++
 } #end while $i lt 5
 ```
 When you run the DemoWhileLessThan.ps1 script, you receive the following output.
 
 ```powershell
 $i equals 0. This is less than  5
$i equals 1. This is less than  5
$i equals 2. This is less than  5
$i equals 3. This is less than  5
$i equals 4. This is less than  5
PS C:\>
```

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/04_Powershell_Scripts/04_Loops_Perf_labs.md" > Continue to Performance Labs </a>
