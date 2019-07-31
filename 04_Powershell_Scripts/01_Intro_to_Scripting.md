<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Scripts

Now that we have learned some useful commands, it would be nice to be able to record a sequence of commands, so that we can execute them all at once. In this chapter, we will learn how to accomplish this with scripts. We will also cover the following topics:

* Execution policies
* Adding parameters to scripts
* Control structures
* Profiles

**Packaging commands**

Saving commands in a file for reuse is a pretty simple idea. In PowerShell, the simplest kind of these files is called a script and it uses the .ps1 extension, no matter what version of PowerShell you're using. For example, if you wanted to create a new folder, under c:\temp, with the current date as the name and then change to that folder, you could put these commands in a file:

Note that I'm using the top portion of the ISE to edit the file contents and I have saved the file as dateFolder.ps1 in the c:\temp folder. To run the script, you can simply type the name of the file at prompt as follows

![image](https://user-images.githubusercontent.com/47218880/61737423-ccb4d180-ad4d-11e9-9e79-dcfa0803c1bc.png)

f you have the file loaded in the ISE, you can also use the Run button on the toolbar to run the current file.

It's possible that when you try to run this script, you will receive an error complaining about the execution policy, instead of seeing the current path being set to a new folder. To understand why this would have happened, we need to discuss the concept of execution policies.


**Execution policy**

Execution policy is a safety feature in PowerShell that enables the system to control which scripts are able to be run. I say safety instead of security because execution policy is trivial to circumvent. Execution policy are more like the safety of a gun, which prevents accidental discharge of the weapon. An execution policy is an attempt to prevent users from accidentally executing scripts.

Possible execution policy values include the following:

* Restricted
* AllSigned
* RemoteSigned
* Unrestricted

The Restricted setting means that the scripts cannot be run at all. AllSigned means that all scripts must be digitally signed to run. Unrestricted says that any script will run. RemoteSigned is a middle-ground setting that says that any local scripts will run, but scripts from remote sources (like the Internet or untrusted UNC paths) need to be digitally signed.

Prior to Windows Server 2012R2, the default execution policy for all systems was Restricted, meaning that by default scripts were not allowed to run at all. With Server 2012R2, the default was changed to RemoteSigned.

Attempting to run a script when the execution policy does not permit it, results in an error such as the following:

![image](https://user-images.githubusercontent.com/47218880/61737580-21f0e300-ad4e-11e9-8445-5294c7f9ba7a.png)

To see what your current execution policy is set to, use the Get-ExecutionPolicy cmdlet. To set the execution policy, you would use the Set-ExecutionPolicy cmdlet.

**TIP**

***Important!***

Since the execution policy is a system setting, changing it requires you to run an elevated session. 
Attempting to change the execution policy from a user session, will result in an "Access Denied" error writing to the registry.

The following figure shows the results of running the script after the execution policy has been set to an appropriate level:

![image](https://user-images.githubusercontent.com/47218880/61737674-549adb80-ad4e-11e9-8d10-9a061cd25e24.png)

In my experience, the RemoteSigned setting is most practical. However, in a secure environment such as a production data center, I can easily see that using an AllSigned policy could make sense.

**Transitioning from command line to script**

Now that you have everything set up to enable script execution, you can run your StopNotepad.ps1 script. This is shown here.

> StopNotepad.ps1

```powershell
Get-Process Notepad | Stop-Process
```

If an instance of the Notepad process is running, everything is successful. 
However, if there is no instance of Notepad running, the error shown here is generated.

```powershell
Get-Process : Cannot find a process with the name 'Notepad'. Verify the process
 name and call the cmdlet again.
At C:\Documents and Settings\ed\Local Settings\Temp\tmp1DB.tmp.ps1:14 char:12
+ Get-Process  <<<< Notepad | Stop-Process
```

![image](https://user-images.githubusercontent.com/47218880/61738335-b0199900-ad4f-11e9-83a4-71cdfaa8fb0e.png)

To make the script easier to read, you break the code at the pipe character. The pipe character is not the line continuation character. The backtick(key to the left of the "1" key) character, also known as the grave accent character, is used when a line of code is too long and must be broken into two physical lines of code. The key thing to be aware of is that the two physical lines form a single logical line of code. An example of how to use line continuation is shown here.

![image](https://user-images.githubusercontent.com/47218880/61738698-7a28e480-ad50-11e9-9c50-b68f84f0f90d.png)

The StopNotepadSilentlyContinue.ps1 script is shown here.

StopNotepadSilentlyContinue.ps1

```powershell
Get-Process -Name Notepad -ErrorAction SilentlyContinue |
Stop-Process
```

Because you are writing a script, you can take advantage of some features of a script. One of the first things you can do is use a variable to hold the name of the process to be stopped. This has the advantage of enabling you to easily change the script to stop processes other than Notepad. All variables begin with the dollar sign. The line that holds the name of the process in a variable is shown here.

```powershell
$process = "notepad"
```

Another improvement you can add to the script is one that provides information about the process that is stopped. The Stop-Process cmdlet returns no information when it is used. However, when you use the -PassThru parameter of the Stop-Process cmdlet, the process object is passed along in the pipeline. You can use this parameter and pipeline the process object to the ForEach-Object cmdlet. You can use the $_ automatic variable to refer to the current object on the pipeline and select the name and the process ID of the process that is stopped. The concatenation operator in Windows PowerShell is the plus sign (+), and you can use it to display the values of the selected properties in addition to the strings that complete your sentence. This line of code is shown here.

```powershell
ForEach-Object { $_.name + ' with process ID: ' +  $_.ID + ' was stopped.'}
```

The complete StopNotepadSilentlyContinuePassThru.ps1 script is shown here.

StopNotepadSilentlyContinuePassThru.ps1

```powershell
$process = "notepad"
Get-Process -Name $Process -ErrorAction SilentlyContinue |
Stop-Process -PassThru |
ForEach-Object { $_.name + ' with process ID: ' +  $_.ID + ' was stopped.'}
```

When you run the script with two instances of Notepad running, output similar to the following is shown.

```
notepad with process ID: 2088 was stopped.
notepad with process ID: 2568 was stopped.
```

An additional advantage of the StopNotepadSilentlyContinuePassThru.ps1 script is that you can use it to stop different processes. You can assign multiple process names (an array) to the $process variable, and when you run the script, each process will be stopped. In this example, you assign the Notepad and the Calc processes to the $process variable. This is shown here.

```powershell
$process = "notepad", "calc"
```

When you run the script, both processes are stopped. Output similar to the following appears.

```powershell
calc with process ID: 3428 was stopped.
notepad with process ID: 488 was stopped.
```

You could continue changing your script. You could put the code in a function, write command-line help, and change the script so that it accepts command-line input or even reads a list of processes from a text file. As soon as you move from the command line to script, such options suddenly become possible.

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/04_Powershell_Scripts/02_Perf_labs.md" > Continue to Performance Labs </a>














