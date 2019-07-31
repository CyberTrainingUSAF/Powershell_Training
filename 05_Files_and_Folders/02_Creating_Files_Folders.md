<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Creating Files and Folders

**New-Item** cmdlet is used to create a directory by passing the path using -Path as path of the directory and -ItemType as Directory.

In this example, we'll create a folder in D:\Temp\ with name "Test Folder"

Type the following command in PowerShell ISE Console

```powershell
New-Item -Path 'D:\temp\Test Folder' -ItemType Directory

```
**Output**

You will see the following output

```
Directory: D:\temp

Mode                LastWriteTime     Length Name                                               
----                -------------     ------ ----                                               
d----          4/3/2018   7:06 PM            Test Folder   
```

You can see the same in Windows Explorer where Test Folder is created in D:/temp folder.

**Create a File**

**New-Item** cmdlet is used to create a file by passing the path using -Path as path of the file and -ItemType as File.

In this example, we'll create a file in D:\Temp\Test Folder with name "Test File.txt"

Type the following command in PowerShell ISE Console

```powershell
New-Item -Path 'D:\temp\Test Folder\Test File.txt' -ItemType File
```

Output
You will see the following output.

```
Directory: D:\temp

Mode                LastWriteTime     Length Name                                               
----                -------------     ------ ----                                               
-a---          4/3/2018   7:14 PM          0  Test File.txt   
```

You can see the same in Windows Explorer where Test File.txt is created in D:/temp/Test Folder directory.

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/05_Files_and_Folders/03_Copying_Files_Folders.md" > Return to TOC </a>
