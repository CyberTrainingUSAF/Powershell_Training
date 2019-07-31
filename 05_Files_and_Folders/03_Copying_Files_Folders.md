<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---
## Cmdlet to use 

**Copy-Item** cmdlet is used to copy a directory by passing the path of the directory to be copied and destination path where the folder is to be copied.

In this example, we'll copy a folder D:\Temp\Test Folder as D:\Temp\Test Folder1

> Type the following command in PowerShell ISE Console

```powershell
Copy-Item 'D:\temp\Test Folder' 'D:\temp\Test Folder1'
```

You can see the Test Folder1 in Windows Explorer created.

In this example, we'll copy a folder recursively D:\Temp\Test Folder to D:\Temp\Test Folder1

* **Type** the following command in PowerShell ISE Console

```powershell
Copy-Item 'D:\temp\Test Folder' -Destination 'D:\temp\Test Folder1'
```

You can see the content of Test Folder1 in Windows Explorer where it contains both the Test Folder and test file.

**Copying a file**

In this example, we'll copy a folder D:\Temp\Test Folder\Test File.txt to D:\Temp\Test Folder1

* Type the following command in PowerShell ISE Console

```powershell
Copy-Item 'D:\temp\Test Folder\Test File.txt' 'D:\temp\Test Folder1\Test File1.txt'
```

You can see the Test File1.txt in Test Folder1 with content of Test File.txt. Test Folder1 folder should be present before running this command.

In this example, we'll copy all text file recursively D:\Temp\Test Folder to D:\Temp\Test Folder1

**Type** the following command in PowerShell ISE Console

```powershell
Copy-Item -Filter *.txt -Path 'D:\temp\Test Folder' -Recurse -Destination 'D:\temp\Test Folder1'
```

You can see the content of Test Folder1 in Windows Explorer where it contains both the Test Folder and only text based file(s).

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/05_Files_and_Folders/04_Deleting_Files_Folders.md" > Continue to Next Topic </a>
