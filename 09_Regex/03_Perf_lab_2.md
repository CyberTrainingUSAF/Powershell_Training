
#  LAB
 

Note
For this lab, you’ll need any computer running PowerShell v3 or higher.

 

Make no mistake about it, regular expressions can make your head spin, so don’t try to create complex regexes right off the bat—start simple. Here are a few exercises to ease you into it. Use regular expressions and operators to complete the following:

Get all files in your Windows directory that have a two-digit number as part of the name.

Find all processes running on your computer that are from Microsoft, and display the process ID, name, and company name. Hint: pipe Get-Process to Get-Member to discover property names.

In the Windows Update log, usually found in C:\Windows, you want to display only the lines where the agent began installing files. You may need to open the file in Notepad to figure out what string you need to select.
