<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Performance Lab Tasks

**HINTS:**

```
Sort-Object
Select-Object
Import-Module
Export-CSV
Help
Get-ChildItem (Dir)
```

* **TASK 1**

  * Run a command that will display the newest 100 entries from the Application event log. Do not use Get-WinEvent.

* **TASK 2**

  * Write a command line that displays only the five top processes based on virtual memory (VM) usage.

* **TASK 3**

  * Create a CSV file that contains all services, including only the service name and status. Have running services listed before stopped services.

* **TASK 4**

  * Write a command line that changes the startup type of the BITS service to Manual.

* **TASK 5**

  * Display a list of all files named win*.* on your computer. 
  * Start in the C:\ folder. **Note:** you may need to experiment and use some new parameters of a cmdlet in order to complete this task.

* **TASK 6**

  * Get a directory listing for C:\Program Files. 
  * Include all subfolders and files. 
  * Direct the directory listing to a text file named  C:\Dir.txt (remember to use the > redirector, or the Out-File cmdlet).

* **TASK 7**

  * Get a list of the 20 most recent entries from the Security event log, and convert the information to XML. Do not create a file on disk: Have the XML display in the console window.
  * Note that the XML may display as a single top-level object, rather than as raw XML data—that’s fine. That’s just how PowerShell displays XML. You can pipe the XML object to Format-Custom to see it expanded out into an object hierarchy, if you like.

* **TASK 8**

  * Get a list of all event logs on the computer, selecting the log name, its maximum size, and overflow action, and then convert to CSV, but without writing to a log file. You may need to discover another CSV related cmdlet.

* **TASK 9**

  * Get a list of services. 
  * Keep only the services’ names, display names, and statuses
  * Send that information to an *html* file with a title of *Service Report*. 
  * Have the phrase *Installed Services* displayed in the *html* file before the table of service information. 
  ```
  * If you can; display installed services with an <H1> html tag. 
  ```
  * Verify the file in a web browser.

* **TASK 10**

  * Create a new alias, named D, which runs Get-ChildItem. Export just that alias to a file. Now, close the shell and open a new console window. Import that alias into the shell. Make sure you can run D and get a directory listing.

* **TASK 11**

  * Display installed hotfixes that are either ‘Hotfix’ or ‘Update’, but not Security Update.

* **TASK 12**

  * Run a command that will display the current directory that the shell is in.

* **TASK 13**

  * Run a command that will display the most recent commands that you have run in the shell. Locate the command that you ran for task 11. Using two commands connected by a pipeline, rerun the command from task 11.

  * In other words, if Get-Something is the command that retrieves historical commands, 5 is the ID number of the command from task 11, and Do-Something is the command that runs historical commands, run this:
  *Get-Something    id 5 | Do-Something*

  Of course, those aren’t the correct cmdlet names—you’ll need to find those. Hint: both commands that you need have the same noun.

* **TASK 14**

  * Run a command that modifies the Security event log to overwrite old events as needed.

* **TASK 15**

  * Use the New-Item cmdlet to make a new directory named C:\Review. This is not the same as running Mkdir; the New-Item cmdlet will need to know what kind of new item you want to create. Read the help for the cmdlet.

* **TASK 16**

  * Display the contents of this registry key:
  * HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders
  * Note: “User Shell Folders” is not exactly like a directory. If you change into that "directory," you won’t see anything in a directory listing. User Shell Folders is an item, and what it contains are item properties. There’s a cmdlet capable of displaying item properties (although cmdlets use singular nouns, not plural).


* **TASK 17**

  * Find (but please do not run) cmdlets that can...
  * Restart a computer
  * Shut down a computer
  * Remove a computer from a workgroup or domain
  * Restore a computer’s System Restore checkpoint

* **TASK 18**

  * What command do you think could change a registry value? Hint: it’s the same noun as the cmdlet you found for task 16.
  
---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/05_Files_and_Folders/01_Working_with_Files.md" > Continue to next topic</a>
