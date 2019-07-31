<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Pre-lab: PowerShell Functions Labs (type and run this script)

```
# func.ps1
# declare :
function add ($a , $b ) {
Write-Host "$a +$b is " ($a +$b )
}
# use :
add 5.12 2.56
```

**Creating a function: Step-by-step exercises**

In this exercise, you’ll explore the use of the Get-Verb cmdlet to find permissible Windows PowerShell verbs. You will also use Function keyword and create a function. After you have created the basic function, you’ll add additional functionality to the function in the next exercise.

**Creating a basic function** 

Start the Windows PowerShell ISE.
Use the Get-Verb cmdlet to obtain a listing of approved verbs.
Select a verb that would be appropriate for a function that obtains a listing of files by date last modified. In this case, the appropriate verb is Get.
Create a new function named Get-FilesByDate. The code to do this is shown here.
```
Function Get-FilesByDate
{

}
```
Add four command-line parameters to the function. The first parameter is an array of file types, the second is for the month, the third parameter is for the year, and the last parameter is an array of file paths. This portion of the function is shown here.
```
Param(
  [string[]]$fileTypes,
  [int]$month,
  [int]$year,
  [string[]]$path)
```
Following the Param portion of the function, add the code to perform a recursive search of paths supplied via the $path variable. Limit the search to include only file types supplied via the $filetypes variable. This portion of the code is shown here.
```
Get-ChildItem -Path $path -Include $filetypes -Recurse |
```
Add a Where-Object clause to limit the files returned to the month of the lastwritetime property that equals the month supplied via the command line, and the year supplied via the command line. This portion of the function is shown here.
```
Where-Object {
   $_.lastwritetime.month -eq $month -AND $_.lastwritetime.year -eq $year }
```
Save the function in a .ps1 file named Get-FilesByDate.ps1.
Run the script containing the function inside the Windows PowerShell ISE.
In the command pane, call the function and supply appropriate parameters for the function. One such example of a command line is shown here.
```
Get-FilesByDate -fileTypes *.docx -month 5 -year 2012 -path c:\data
```
The completed function is shown here.
```
Function Get-FilesByDate
{
 Param(
  [string[]]$fileTypes,
  [int]$month,
  [int]$year,
  [string[]]$path)
   Get-ChildItem -Path $path -Include $filetypes -Recurse |
   Where-Object {
   $_.lastwritetime.month -eq $month -AND $_.lastwritetime.year -eq $year }
  } #end function Get-FilesByDate
 ```
This concludes this step-by-step exercise.

In the following exercise, you will add additional functionality to your Windows PowerShell function. In this additional functionality you will include a default value for the file types and make the $month, $year, and $path parameters mandatory.

**Adding additional functionality to an existing function**

Start the Windows PowerShell ISE.
Open the Get-FilesByDate.ps1 script (created in the previous exercise) and use the Save As feature of the Windows PowerShell ISE to save the file with a new name of Get-FilesByDateV2.ps1.
Create an array of default file types for the $filetypes input variable. Assign the array of file types to the $filetypes input variable. Use array notation when creating the array of file types. For this exercise, use *.doc and *.docx. The command to do this is shown here.
```
[string[]]$fileTypes = @(".doc","*.docx"),
```
Use the [Parameter(Mandatory=$true)] parameter tag to make the $month parameter mandatory. The tag appears just above the input parameter in the param portion of the script. Do the same thing for the $year and $path parameters. The revised portion of the param section of the script is shown here.
```
[Parameter(Mandatory=$true)]
  [int]$month,
  [Parameter(Mandatory=$true)]
  [int]$year,
  [Parameter(Mandatory=$true)]
  [string[]]$path)
```
Save and run the function. Call the function without assigning a value for the path. An input box should appear prompting you to enter a path. Enter a single path residing on your system, and press Enter. A second prompt appears (because the $path parameter accepts an array). Simply press Enter a second time. An appropriate command line is shown here.
```
Get-FilesByDate -month 10 -year 2011
```
Now run the function and assign a path value. An appropriate command line is shown here.
```
Get-FilesByDate -month 10 -year 2011 -path c:\data
```
Now run the function and look for a different file type. In the example shown here, I look for Microsoft Excel documents.
```
Get-FilesByDate -month 10 -year 2011 -path c:\data -fileTypes *.xlsx,*.xls
```
The revised function is shown here.
```
Function Get-FilesByDate
{
 Param(
  [string[]]$fileTypes = @(".DOC","*.DOCX"),
  [Parameter(Mandatory=$true)]
  [int]$month,
  [Parameter(Mandatory=$true)]
  [int]$year,
  [Parameter(Mandatory=$true)]
  [string[]]$path)
   Get-ChildItem -Path $path -Include $filetypes -Recurse |
   Where-Object {
   $_.lastwritetime.month -eq $month -AND $_.lastwritetime.year -eq $year }
  } #end function Get-FilesByDate
  ```
This concludes the exercise.

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/04_Powershell_Scripts/08_Modules.md" > Continue to Next Topic </a>
