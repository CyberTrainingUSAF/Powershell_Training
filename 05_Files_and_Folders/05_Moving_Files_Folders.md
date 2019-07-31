<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Moving Files and Folders

**Move-Item** cmdlet is used to move a directory by passing the path of the directory to be moved and destination path where the folder is to be moved.

In this example, we'll move a folder D:\Temp\Test to D:\Temp\Test1

**Type** the following command in PowerShell ISE Console

```powershell

Move-Item D:\temp\Test D:\temp\Test1

```
You can see the Test directory moved to Test1 directory in Windows Explorer.

In this example, Create a file test.txt in Test folder in D:\Temp\ and then run the same command.

**Type** the following command in PowerShell ISE Console

```powershell

Move-Item D:\temp\Test D:\temp\Test1

```

The content of Test1 directory is in Windows Explorer where it contains both the Test directory and test file.

Move-Item cmdlet is used to move a file by passing the path of the file to be moved and destination path where the file is to be moved.

---

* Example 1
  * Move a folder D:\Temp\Test\Test.txt to D:\Temp\Test1

**Type** the following command in PowerShell ISE Console

```powershell
Move-Item D:\temp\Test\Test.txt D:\temp\Test1
```

The Test.txt is now in the Test1 directory. Test.txt has been moved from Test to Test1.

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/06_Powershell_ErrorHandling/06_Powershell_ErrorHandling.md" > Continue to Next Topic </a>
