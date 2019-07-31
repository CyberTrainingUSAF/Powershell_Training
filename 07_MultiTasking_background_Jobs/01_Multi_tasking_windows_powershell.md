<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Multitasking Windows PowerShell

You may have noticed in your Windows PowerShell practice thus far that you temporarily lose control over your session when you execute one or more commands. This behavior is by design; you give the shell a set of instructions, and it goes about its business until it’s finished, after which you get your prompt back.

Wouldn’t it be nice to send commands to PowerShell to process in the background? Think of how this would enhance your productivity! You send a script to the shell, get your prompt back, and then you’re free to, say, send another script to the shell.

Naturally, you could spawn multiple Windows PowerShell sessions to do this, but as it turns out, this approach is unnecessary.

Windows PowerShell includes a built-in job architecture that enables us to get PowerShell truly multitasking. By the end of this section, you’ll understand all about how the jobs system works.

---

**INVESTIGATING THE POWERSHELL JOB ARCHITECTURE**

The UNIX/Linux operating system family has long had the ability to send shell commands to the background for “behind the scenes” processing. Thanks to the Windows PowerShell team, we now also have this ability.

* Let’s begin by seeing what job-related cmdlets are available to us:

```powershell

PS C:\> Get-Command –Noun Job



CommandType     Name

-----------     ----

Cmdlet          Debug-Job

Cmdlet          Get-Job

Cmdlet          Receive-Job

Cmdlet          Remove-Job

Cmdlet          Resume-Job

Cmdlet          Start-Job

Cmdlet          Stop-Job

Cmdlet          Suspend-Job

Cmdlet          Wait-Job
```
By now, you should be familiar enough with the common PowerShell verbs so as to have a grasp of what you can do with these job cmdlets.

---

**Starting a Background Job**

Windows PowerShell is a bit quirky in spots, I’m sure you’d agree with me. This statement applies to how these PowerShell background jobs run under the hood. You’ll see this in Technicolor when I tell you about parent and child jobs later in this section.

For now, let’s create a new job that searches our E: drive for all Adobe Acrobat PDF files:

```powershell

$gci = Start-Job –ScriptBlock { Get-ChildItem –Path "E:\" –Filter *.pdf –Recurse }
```

* First of all, I invite you to try that Get-ChildItem command without using a job; you’ll find that your PowerShell session locks up for an uncomfortably long time. However, when you submit a job, you can immediately resume your work in the shell.

* Second, notice that I saved the job to a variable. This is a habit that you should adopt concerning jobs. In my experience it’s much easier to work with jobs if you store the job object in an easy-to-remember, brief variable name.

---

**Checking Job Status**

Now, let me walk you through how we can check on job status and comprehend the output:

```powershell

PS C:\> Get-Job



Id     Name            PSJobTypeName   State         HasMoreData     Location

--     ----            -------------   -----         -----------     --------

2      Job2            BackgroundJob   Running       True            localhost

```
Of course, Get-Job run with no parameters retrieves all existent jobs on the local system. If we know the name, ID, or variable name of a job, we can leverage that in the following ways:
```powershell

Get-Job –Id 2

Get-Job –Name Job2

$gci | Get-Job
```

Incidentally, notice in that output that Windows PowerShell automatically assigns incrementing names to your jobs. In addition to or instead of using a variable, you can specify a friendly name:

```powershell

PS C:\> $gs = Start-Job -Name "Antimalware" -Scriptblock {Get-Service | Where-Object { $_.Name -like "*malware*"} | Stop-Service}

PS C:\> $gs



Id     Name            PSJobTypeName   State         HasMoreData     Location

--     ----            -------------   -----         -----------     --------

4      Antimalware     BackgroundJob   Completed     False           localhost


```

---

**Understanding Job Status Output**

Okay, enough with the fun and games. We need to understand what each of those output fields means. Let’s get to it:

I want to draw your attention to the HasMoreData property. If this property returns Boolean true, that means that PowerShell is holding the job results in RAM for you and you haven’t yet seen it.

---

**Retrieving Job Data**

Let’s return to the original example:

```powershell

PS C:\> $gci = Start-Job -ScriptBlock {Get-ChildItem -Path "E:\" -Filter *.pdf

PS C:\> Get-Job



Id     Name            PSJobTypeName   State         HasMoreData     Location

--     ----            -------------   -----         -----------     --------

2      Job2            BackgroundJob   Running       True            localhost
```

You can view the output of a job, even one that is still running, by using Receive-Job:
```powershell

Receive-Job –Id 2
```

Again, you can use the name, ID, or variable name, depending on your preference.

The gotcha with Receive-Job is that unless you specify the –Keep parameter, viewing completed job output dumps it all from memory. I’m not kidding. To test this, wait until your PDF search job completes, and then run a Receive-Job on it. Look here:

```powershell


PS C:\> Get-Job



Id     Name            PSJobTypeName   State         HasMoreData     Location

--     ----            -------------   -----         -----------     --------

2      Job2            BackgroundJob   Completed     True           localhost


```
```powershell

PS C:\> Receive-Job –Name "Job2"

```
```powershell

#output omitted to save space



PS C:\> Get-Job



Id     Name            PSJobTypeName   State         HasMoreData     Location

--     ----            -------------   -----         -----------     --------

2      Job2            BackgroundJob   Completed     False           localhost



PS C:\> Receive-Job -Id 2

PS C:\>
```

**Ouch!** That hurts. Notice that PowerShell changes the HasMoreData property value to False once it’s dumped the job results from memory. By contrast, whenever we do either of the following (other command variants are possible, naturally):

```powershell


Receive-Job –Id 2 –Keep

Get-Job –Id 2 | Receive-Job -Keep
```
we can retrieve the job results as many times as we want to, and PowerShell will keep the job and the output in memory until the session is closed or we remove the job. By the way, you know that the job data is being retained in memory whenever you see the HasMoreData job property is set to True.

**Tip**

Get into the habit of using the –Keep parameter whenever you use Receive-Job. Remember that we use Windows PowerShell to make our work faster and more convenient; we never want to create unnecessary and excess work for ourselves or others.

I want you to know that Start-Job accepts entire PowerShell scripts as input. The –FilePath parameter is excellent when you have a long-running script that you don’t want to tie up your current session. Here’s an example of its usage:

```powershell


Start-Job –Name TaskScript –FilePath "C:\scripts\pstasklist.ps1"
```

---

**CONTROLLING JOB BEHAVIOR**

At this point you know how to define a job, fetch its status, and receive its output. In the next sections, we’ll learn how to stop, resume, and delete jobs.

Stopping a Job
Okay, let’s put a couple jobs into the queue and fetch their status:

```powershell


PS C:\> $txt = Start-Job -ScriptBlock {Get-ChildItem -Path "C:\" -Filter *.txt -Recurse}

PS C:\> $png = Start-Job -ScriptBlock {Get-ChildItem -Path "C:\" -Filter *.png -Recurse}

PS C:\> Get-Job



Id     Name            PSJobTypeName   State         HasMoreData     Location

--     ----            -------------   -----         -----------     --------

2      Job2            BackgroundJob   Running       True            localhost

4      Job4            BackgroundJob   Running       True            localhost

```

You probably noticed that the system-generated ID values don’t increment exactly by one. For instance, my first job has ID 2, and the second job has ID 4. What’s the deal? Don’t worry; we’ll discuss parent and child jobs soon enough.

More immediately, however, we have the problem that these two resource-intensive jobs are running concurrently and potentially slowing down our system. Let’s stop the first job:
```powershell

Stop-Job –Id 2
```
There. If you run another Get-Job, you’ll observe that this first job now shows a state of Stopped. The True value for HasMoreData means that we can retrieve the job results as usual. Of course, the job results will contain the data from the start of the job until the moment you stopped it.

Alternatively, we can supply a comma-separated list of ID or Name values to Stop-Job:

S```powershell
top-Job –Id 2,4
```
Resuming a Stopped Job
I’m sorry to say that once a PowerShell background job is stopped, you can’t simply pipe it to Start-Job, however logical that approach may seem to you.

As proof, let’s fetch the methods for a stopped job on my test system:

```powershell


PS C:\> Get-Job -id 2 | Get-Member -MemberType Method



   TypeName: System.Management.Automation.PSRemotingJob



Name             MemberType Definition

----             ---------- ----------

Dispose          Method     void Dispose(), void IDisposable.Dispose()

Equals           Method     bool Equals(System.Object obj)

GetHashCode      Method     int GetHashCode()

GetType          Method     type GetType()

LoadJobStreams   Method     void LoadJobStreams()

StopJob          Method     void StopJob()

ToString         Method     string ToString()

UnloadJobStreams Method     void UnloadJobStreams()
```
***Caution***

Notice that we have a StopJob() method but nothing to restart it. Therefore, the takeaway here is don’t stop a job unless you actually want to stop it permanently.

Stopped and completed jobs remain in the Get-Job queue until either the session is closed or you manually purge the queue.

If you need to pause a job for later resumption, you should use the Suspend-Job and Resume-Job cmdlets, respectively. However, those cmdlets are intended for Windows PowerShell workflow jobs, and we haven’t arrived at workflows yet. Stay tuned for that content!

---

**Deleting Jobs from the Queue**

If you need to “nuke” jobs from the queue, we bring Remove-Job into action. Here are three examples of its usage. (You should know the proverbial drill by now.)

```powershell

Remove-Job –Id 6

Remove-Job –Name MyJob

$job | Remove-Job

Remove-Job –State Failed
```

This last example is interesting. The –State property exists in most of the Job-related cmdlets. Here’s a comprehensive listing of the –State enumeration:

 NotStarted

 Running

 Completed

 Failed

 Stopped

 Blocked

 Suspended

 Disconnected

 Suspending

 Stopping

If you need to swing the so-called heavy hammer, you can run either of the following commands to remove every job in the queue, regardless of its status:

```powershell

Get-Job | Remove-Job

Remove-Job *
```

---

The power instruction is complete.  PLease do not stop practicing using this scripting tool.  There is no better way to learn than to take the knowledge provided, and use that to start your own learning effort.   Good Luck!

---

<a href="https://github.com/CyberTrainingUSAF" > Return to Cyber Training page </a>
