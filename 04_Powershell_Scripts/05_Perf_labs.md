<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## CREATING MULTIPLE FOLDERS: STEP-BY-STEP EXERCISES

In the first exercise, you’ll explore the use of constants, variables, concatenation, decision-making, and looping as you create 10 folders in the C:\mytempfolder directory. This directory was created earlier. If you do not have this folder on your machine, you can either create it manually or modify the following two exercises to use a folder that exists on your machine. In the second exercise in this section, you will modify the script to delete the 10 folders

> Creating multiple folders by using Windows PowerShell scripting

1. Open the Windows PowerShell ISE.

2. Create a variable called $intFolders and have it hold the value 10. The code to do this is shown here.
```
$intFolders = 10
```
3. Create a variable called $intPad. Do not put anything in the variable yet. This code is shown here.
```
$intPad
```
4. Create a variable called $i and put the value 1 in it. The code to do this is shown here.
```
$i = 1
```
5. Use the New-Variable cmdlet to create a variable named strPrefix. Use the -Value parameter of the cmdlet to assign a value of testFolder to the variable. Use the -Option parameter to make $strPrefix into a constant. The code to do this is shown here.
```
New-Variable -Name strPrefix -Value "testFolder" -Option constant
```
6. Begin a Do...Until statement. Include the opening brace for the script block. This code is shown here.
```
do {
```
7. Begin an If...Else statement. The condition to be evaluated is if the variable $i is less than 10. The code that does this is shown here.
```
if ($i -lt 10)
```
8. Open the script block for the If statement. Assign the value 0 to the variable $intPad. This is shown here.
```
{$intPad=0
```
9. Use the New-Item cmdlet to create a new folder. The new folder will be created in the C:\mytempfolder directory. The name of the new folder will be made up of the $strPrefix constant testFolder, the number 0 from the $intPad variable, and the number contained in the $i variable. The code that does this is shown here.
```
New-Item -Path c:\mytempfolder -Name $strPrefix$intPad$i -Type directory}
```
10. Add the Else clause. This code is shown here.
```
else
```
11. The Else script block is the same as the If script block, except it does not include the 0 in the name that comes from the $intPad variable. Copy the New-Item line of code from the If statement and delete the $intPad variable from the -Name parameter. The revised line of code is shown here.
```
{New-Item -Path c:\mytempfolder -Name $strPrefix$i -Type directory}
```
12. Increment the value of the $i variable by one. To do this, use the double-plus symbol operator (++) . The code that does this is shown here.
```
$i++
```
13. Close the script block for the Else clause and add the Until statement. The condition that Until will evaluate is whether the $i variable is equal to the value contained in the $intFolders variable + 1. The reason for adding 1 to $intFolders is so the script will actually create the same number of folders as are contained in the $intFolders variable. Because this script uses a Do...Until loop and the value of $i is incremented before entering the Until evaluation, the value of $i is always 1 more than the number of folders created. This code is shown here.
```
}until ($i -eq $intFolders+1)
```
14. Save your script as <yourname>CreateMultipleFolders.ps1. Run your script. You should find 10 folders created in the C:\mytempfolder directory. This concludes this step-by-step exercise.


## Deleting multiple folders

1. Open the <yourname>CreateMultipleFolders.ps1 script created in the previous exercise in the Windows PowerShell ISE.

2. In the If...Else statement, the New-Item cmdlet is used twice to create folders in the C:\mytempfolder directory. You want to delete these folders. To do this, you need to change the New-Item cmdlet to the Remove-Item cmdlet. The two edited script blocks are shown here.
```
{$intPad=0
      Remove-Item -Path c:\mytempfolder -Name $strPrefix$intPad$i -Type directory}
   else
      {Remove-Item -Path c:\mytempfolder -Name $strPrefix$i -Type directory}


```
3. The Remove-Item cmdlet does not have a -Name parameter. Therefore, you need to remove this parameter but keep the code that specifies the folder name. You can basically replace -Name with a backslash, as shown here.
```
{$intPad=0
      Remove-Item -Path c:\mytempfolder\$strPrefix$intPad$i -Type directory}
   else
      {Remove-Item -Path c:\mytempfolder\$strPrefix$i -Type directory}
```
4. The Remove-Item cmdlet does not take a -Type parameter. Because this parameter is not needed, it can also be removed from both Remove-Item statements. The revised script block is shown here.
```
{$intPad=0
     Remove-Item -Path c:\mytempfolder\$strPrefix$intPad$i}
   else
    {Remove-Item -Path c:\mytempfolder\$strPrefix$i}

```

5. This concludes this exercise. Save your script as <yourname>DeleteMultipleFolders.ps1. Run your script. The 10 previously created folders should be deleted.
  
![image](https://user-images.githubusercontent.com/47218880/61742185-2b7f4880-ad58-11e9-90e7-04db7d6bf718.png)

## CHALLENGE LAB 
Start up four instances of Windows Notepad on your system. By using only Windows PowerShell, learn everything you can about the processes. Determine whether you can kill all four processes at once.

Similarly, figure out a way to get a list of running services on your system, sort them alphabetically by service name, and output to an HTML report. We haven’t covered all the required steps yet; I’m challenging you to use your PowerShell-fu and research skills to get the job done. I believe in you.

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/04_Powershell_Scripts/06_Powershell_Functions.md" > Continue to Next Topic </a>
