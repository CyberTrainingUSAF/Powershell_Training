<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

## **Working With PowerShell Remoting Lab**

To complete these steps, you need two computers that are joined to an Active Directory domain and enabled for Windows PowerShell remoting. Although we won’t undertake any destructive actions, be careful if you attempt these steps on a production network.

This exercise assumes that you’re working from a Windows Server 2012 R2 member server named MEM1 and that you want to perform remote operations on a Windows Server 2012 R2 domain controller named DC1. Of course, you’ll substitute hostname values for your own computers’ hostnames. Let’s begin:

1. We begin by verifying that DC1, our remote host, is open for business. Instead of ping, we can leverage the Test-Connection Windows PowerShell cmdlet:
```powershell
Test-Connection dc1
```
2. Let’s obtain the BIOS version from our remote host. Are we using WS-Man remoting here or no? How can you tell?

```powershell
Get-WMIObject –ComputerName dc1 –Class Win32_BIOS
```
3. Next, we’ll look at the error messages from the remote system’s System log, formatted as a list for easier reading.

```powershell
Invoke-Command –ComputerName dc1 –ScriptBlock {Get-EventLog –LogName System | Where-Object {$_.EntryType –eq "Error"} | Format-List}
```
4. If we’re going to be working with DC1 for a while, why not store the remote session persistently in a variable?
```powershell
$remote = New-PSSession –ComputerName dc1
```
Make sure to verify that the remote session is active and in an opened state:
```powershell
$remote
```
5. Now it’s time to enter that session and execute a remote command directly from the context of the remote computer:

```powershell
Enter-PSSession –Session $remote
Get-Process | Where-Object {$_Status –eq "Running"} | Export-Csv –Path \\mem1\c$\dc1-runningservices.csv
```
Do you see what we’ve done? We captured the running services on DC1, created a CSV report, and placed it on our local computer’s C: drive. Awesome.

6. We can now verify that the file exists and scan its contents:
```powershell
Exit-PSSession
Notepad "c:\runningservices.csv"
```
7. Let’s imagine that you need to disconnect the remote session from your local computer and you plan to resume the session from yet another computer. First, make a note of the session name:
```powershell
Get-PSSession
```
Second, we’ll disconnect:
```powershell
Get-PSSession –Name "SessionName" | Disconnect-PSSession
```
8. Then, from the same or another computer in the same domain, we can actually pick up the session again.

Run Get-PSSession to get the session name for the disconnected session, and then pick up where you left off:
```powershell
Enter-PSSession –Name "SessionName"
```
9. When you’re finished with the session, destroy the remote session entirely:
```powershell
Get-PSSession –Name "SessionName" | Remove-PSSession
```

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/Managing_Computers_Remotely/One_to_Many_Remoting.md" > Continue to Next Topic </a>
