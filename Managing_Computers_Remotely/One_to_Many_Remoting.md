<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## **One-To-Many Remoting in the Classic Scenario** 

We can take a look at the -ComputerName help file and see that we can enter multiple names, or a comma separated array.

```powershell
Get-Help Get-WmiObject -Parameter ComputerName
```

Lets specify a command to multiple computernames.

```powershell
Get-WMIObject -ComputerName mem1, mem2 -Class Win32_BIOS
```

We can also take a file containing a one-column listing of any number of computer names, extract its context, and feed the string as an array to the -ComputerName parameter

```powershell
Get-WMIObject -Computername (Get-Content "C:\servernames.txt") -Class Win32_BIOS
```

## **One-To-Many Remoting with Persistent Sessions**

We can begin by creating a variable to hold our two remote servers and verifying that the object was successfully created:

```powershell
$s = New-PSSession -ComputerName mem1, mem2
$s
```

Next, we can use the Invoke-Command to enumerate the contents of the Run Registry key. We can use the Get-ItemProperty cmdlet

```powershell
Invoke-Command -Session $s -ScriptBlock {Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run"}
```

### **Working with Disconnected Sessions**

The disconnected sessions feature in Windows PowerShell is very powerful. Lets look at the **Disconnect-PSSession** and **Connect-PSSession** cmdlets. 

Suppose you created a psersistent remote session to three servers named DC1, DC2, and DC3 to do some work.

```powershell
$s = New-PSSession -ComputerName dc1, dc2, dc3 -Name ITTask -Credential domain\tech

Enter-PSSession -Session $s
```

You're not sure how to handle some strange errors that you're receiving. You can call your boss and have them log into the session after you disconnect from the session. Remember disconnect not remove.

```powershell
Exit-PSSession

Get-PSSession -Name ITTask | Disconnect-PSSession
```

The boss can now connect to the very same session.

```powershell
Get-PSSession -ComputerName dc1, dc2, dc3 -Name ITTask

$admin = Connect-PSSession -ComputerName DC1 -Name ITTask domain\administrator
```


## **Managing Session Configurations**

In PowerShell we can send commands or script files to up to 32 remote computers simultaneously by default. This is parallel processing so all 32 machines are remotely executing your code at the same time.

You can change this limit by using the -ThrottleLimit parameter in your Invoke-Command statement.

```powershell
$fanout = New-PSSession -ComputerName (Get-Content "C:\scripts\computers.txt")

Invoke-Command -Session $fanout -FilePath "C:\scripts\config.ps1" -ThrottleLimit 75
```
This means that config.ps1 will be processed in parallel by 75 computers simultaneously.


<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/Managing_Computers_Remotely/Lab_One_to_Many.md" > Continue to Next Topic </a>
