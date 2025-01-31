<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Error Handling

Error handling in powershell is necessary for all sorts of different types of scripts. You want to make sure that you have something in place to handle an automated script, or something that handles an error you may run into while looping through a file structure. You could build a test script that runs fine but when thrown into production you run into an error that you didn't account for, you don't want your script to abruptly stop, you want to be able to handle these issues. 

Imagine that you are removing a list of files from your system. It turns out that, for whatever reason, you can't delete one of the files. Do you want your cmdlet to stop running as soon as it hits the first of these occurences, or do you want your script to continue on and log the errors to a list that you can review for later?

**Note:** This lesson will be conducted in the Powershell ISE

---

**Terminating vs Non-Terminating**

The first thing that we have to understand is that there are two types of errors that we will run into, Terminating errors and Non-Terminating errors.

**Terminating**
Terminating errors halt anything that is running in the pipeline. Essentially they will stop the script from continuing on and output the error to the screen. These are commonly syntax errors, cmdlets that do not exist, and other fatal errors.

**Non-Terminating**
Non-Terminating errors do not stop the pipeline from executing. Essentially they will output the error to the screen and continue on with the script. Common examples are things like file not found, or permission issues. 

---

**The $error variable**

The $error variable contains a collection of errors while you have a powershell instance open. This variable can hold a maximum count of 256 - 32768 errors. The default maximum is 256 and you will rarely want to change it to hold more errors. To double check or change the default you can look at the $MaximumError-Count. 

The $error variable is an array of System.Collections.ArrayList which means that you can access the collection by index, with the most recent error being located at $error[0]. 

**Exploring the $error variable.**

In a **NEW** powershell instance type 
```powershell
$error
```
You will notice nothing happens and that is because you have a new instance of powershell open with no errors assigned to the $error variable.

![](/Assets/error.png)

Now type 
```powershell
Get-ThisIsNotAValidNoun  
```
to get an invalid cmdlet error. You should notice an error like below.

![](/Assets/thisisnotanoun.png)

Now we can access this error and see that it is part of the $error variable by typing 
```powershell
$error[0]
```

![](/Assets/errorZero.PNG)

Add another error on purpose by typing another noun that doesn't exist. Something like *Get-AnotherInvalidNoun*

Now you can see that if you just type 
```powershell
$error
```
 then it dumps every error in the list. Again if you type *$error[0]* then you get the most recent error like how we stated earlier. *$error[1]* is now our original error. 

![](/Assets/multipleErrors.png)

Try 
```powershell
$error.count
``` 
and see if it makes sense.  

You should see 2 at this point. This is useful if you don't want to dump all of the $errors but need to see how many errors are in your variable. 


**Looking more closely at $error**

The ErrorRecord is a rich object that has many useful properties that you can explore. The two main properties that we are going to focus on are **InvocationInfo** and **Exception**. If you'd like to explore more than this on your own then refer to the documents [here](https://docs.microsoft.com/en-us/dotnet/api/system.management.automation.errorrecord?redirectedfrom=MSDN&view=pscore-6.2.0)

Try typing the following:
```powershell
$error[0] | Get-Member
```
![](/Assets/errorGetMember.png)

**InvocationInfo**

We can see that we have the option to use the property 
```powershell
$error[0].InvocationInfo
```
This reveals the line that the command was called on in our script. For these examples we've been creating an error on a single line but you can see how this can be useful to pinpoint exactly where you are getting the error in a script with more than one line.

```powershell
$error[0].InvocationInfo.Line
```
![](/Assets/errorInvocationInfo.PNG)

**Exception**

With the earlier command of *$error[0] | Get-Member* we can also see that we have an Exception property. We can use this property and get more information about it to figure out the TypeName of our Exception which is really important when we get into catching errors.
```powershell
$error[0].Exception | Get-Member
```
![](/Assets/errorTypeName.PNG)

You can see in the screen shot above the TypeName. We will come back to this after we touch on controlling actions on errors.

*TypeName: System.Management.Automation.CommandNotFoundException*

**Error Action Preference**

Sometimes we run into critical issues in our logic with non-terminating errors. In these cases we want to treat our non-terminating error like a terminating error. An example of this would be if we're reusing a directory that someone else was previously using. The first step we want to do is remove all of the files from the directory before installing the new files. We can't start installing the new files if the old ones are not deleted correctly. Failure to delete a file is normally a non-terminating error but in this case we need to treat it like a terminating error. We can control this by setting the error action preference. 

There are five values we can choose for our error action.

* **Stop**
    * Display error, stop execution
    
* **Inquire**
    * Display error, ask to continue
    
* **Continue (Default)**
    * Default setting. Display error, continue execution
    
* **Suspend**
    * This is only for workflows. Automatically suspends the workflow job to all for further investigation, then workflow can be resumed.
    
* **SilentlyContinue**
    * Suppress error message and continue executing command.
    
* **Ignore (Introduced in Powershell 3.0)**
    * Similar to SilentlyContinue but does not add the error to $error variable

We can set this property in two ways.

The first way is to set it globally in our Powershell instance by using the variable $ErrorActionPreference. The second way is to use the -ErrorAction parameter (alias of -ea) that is available for all cmdlets. 

We are going to try to change directories to a madeup location and then write out to the host.

Type out the following:

```powershell
dir C:\MadeupFolder; Write-Host 'This still writes'
```

![](/Assets/errorStillWrites.PNG)

You should notice that we get an ItemNotFoundException but our second cmdlet still runs and we see the message 'This still writes'. Currently our $ErrorActionPreference is set to Continue by default so it outputs the error and continues execution. 

Now lets change our $ErrorActionPreference to Stop and we shouldn't see the second Write-Host command go through and won't see 'This still writes'.

```powershell
$ErrorActionPreference = 'Stop'
dir C:\MadeupFolder; Write-Host 'This still writes'
```

![](/Assets/EAStop.PNG)

or we can set the -ErrorAction parameter and see the same

```powershell
dir C:\MadeupFolder; Write-Host 'This still writes' -ErrorAction "Stop"
```

![](/Assets/EAStop2.PNG)

Every cmdlet has access to this parameter. This means that we can read more about it in the docs by looking at CommonParameters and scrolling down to -ErrorAction. 

```powershell
help about_commonParameters
```

![](/Assets/commonParameters.PNG)

---

**Try/Catch/Finally**

While scripting you'll want to be sure that you're handling and capturing any errors that you could run into. This is where the try/catch/finally statement comes into use. The **try** block is always required along with at least one of the **catch** or **finally** clauses.

AN **IMPORTANT** NOTE - Catch will not catch non-terminating errors. So in order for our non-terminating errors to behave how we want we have to set our $ErrorActionPreference = 'Stop' like before.

If you have a finally block, it will always run. 

We can see the TypeName of our error like we did earlier and use that in our catch block.

```powershell
$error[0].Exception | Get-Member
```
We see that we have an ItemNotFoundException.

![](/Assets/itemNotFoundException.PNG)

or we can call the GetType() method to get the full name.

```powershell
$error[0].Exception.GetType().FullName
```

![](/Assets/exceptionFullName.PNG)

With this we can write our Try/Catch/Finally block and run the script by pressing the green play button in our Powershell ISE toolbar.

```powershell
Try {
    dir C:\MadeupFolder
}

Catch [System.Management.Automation.ItemNotFoundException]{
    "Caught the Exception"
}
Finally {
    "Finally Block Reached"
}
```

![](/Assets/tryCatchFinal.PNG)

As you can see since the ItemNotFoundException is a non-terminating error, the **Catch** block is not ran. If we change our $ErrorActionPreference = 'Stop' then we will see "Caught the Exception" printed to the screen.

![](/Assets/tryCatchEAStop.PNG)

---
This is just scratching the surface of error handling. I've given you the tools and examples necessary to branch out and start experimenting with how you can utilize the $ErrorActionPreference and make sure your scripts do not go into production without proper failsafes and error handling. 

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/07_MultiTasking_background_Jobs/01_Multi_tasking_windows_powershell.md" > Continue to Next Topic </a>
