<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Deleting Files and Folders

**Remove-Item** cmdlet is used to delete a directory by passing the path of the directory to be deleted.

In this example, we'll delete a folder D:\Temp\Test Folder1

**Type** the following command in PowerShell ISE Console

```powershell
Remove-Item 'D:\temp\Test Folder1'
```

You can see the Test Folder1 in Windows Explorer is deleted now.

In this example, we'll remove the folder D:\Temp\Test Folder1 recursively. In first example, PowerShell confirms if directory is not empty. In this case, it will simply delete the item.

**Type** the following command in PowerShell ISE Console

```powershell
Remove-Item 'D:\temp\Test Folder' -Recurse
```

You can see the content of temp folder in Windows Explorer where its folders are now removed.

**Remove-Item** cmdlet is used to delete a file by passing the path of the file to be deleted.

In this example, we'll delete a file D:\Temp\Test Folder\Test.txt

**Type** the following command in PowerShell ISE Console

```powershell
Remove-Item 'D:\temp\Test Folder\test.txt'
```

You can see the Test Folder1 in Windows Explorer is deleted now.

In this example, we'll remove the folder D:\Temp\Test Folder recursively deleting its all files. In first example, PowerShell confirms if directory is not empty. In this case, it will simply delete the item.

**Type** the following command in PowerShell ISE Console

```powershell
Remove-Item 'D:\temp\Test Folder' -Recurse
```

You can see the content of temp folder in Windows Explorer where its folder is now removed.

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/05_Files_and_Folders/05_Moving_Files_Folders.md" > Continue to the Next Topic </a>
