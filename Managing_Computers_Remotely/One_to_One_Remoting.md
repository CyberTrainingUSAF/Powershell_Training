<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## **Understanding Classic Windows Powershell Remote Access** 

You may be familiar with the following command that uses -ComputerName to send commands to remote hosts and receive their outputs but this technique isn't "PowerShell" remoting.

```powershell
Get-Command -CommandType Cmdlet -ParameterName ComputerName | Select-Object -Property Name
```
![](/Assets/GetCommandComputerName.png)

Why isn't this really "PowerShell" remoting? There is no shell-wide, universal remoting architecture at play yet.


You can use this technique for more direct approaches but here are some challenges of using -ComputerName
* ComputerName uses proprietary, legacy network protocols such as Distributed Component Object Model (DCOM) and Remote Procedure Calls (RPCs)
* These legacy protocols connect using TCP and UDP, which means they run into problems on firewalled networks
* If you send these remote commands to multiple computers they are done serially, one host at a time, instead of in parallel like modern PowerShell

Howerver, using the -ComputerName method is good for any quick and dirty remoting tasks that you'd like to perform if you get over the Windows Firewall issue mentioned above. 

```powershell
Get-Service -ComputerName server01
```

When you start to incorporate multiples hosts then you start to lose any advantage that the -ComputerName method has.

![](/Assets/GetServiceMultiple.png)

If you do this method you can't see which server corresponds to which service name.

## **Introducing True PowerShell Remoting**

Starting with PowerShell v2, Microsoft added true remote management capability to Windows PowerShell. Instead of employing custom code on a per-cmdlet basis, we have a single, unified remote access method that works across the entire shell. PowerShell now uses the industry standard Web Services-Management protocol (WS-Man), which communicates over HTTP(S) and uses only 1-2 communications ports. 

The image below shows how Windows PowerShell remoting works in a nutshell.

* Any commands you send from your computer are executed on the remote hosts.
* The data transmitted across your network consists of deserialized XML content
* The output that is returned from the remote systems is reconstituted back into full-fidelity object data
* The remote output represents a point-in-time snapshot of the remote systems


![](/Assets/powershellremoting.png)

## WS-Man and WinRM

Windows PowerShell remoting employs the industry standard WS-Man protocol. Two communications ports are required for remoting to work: TCP port 5985 for unencrypted HTTP, and TCP port 5986 for encrypted HTTPS.

Any local-area network should allow HTTP/HTTPS traffic. 

Windows Remote Management is the underlying Windows service that controls the behavior of the WS-Man protocol. Enabling PowerShell remote management involves a few key tasks:
* Start the WinRM service
* Configure the WinRM service to autostart at boot
* Create an HTTP or HTTPS listener
* Enable the appropriate Windows Firewall exceptions  

How do you do all of these steps? PowerShell can perform all of these remote setup tasks in a single cmdlet!

## Enabling PowerShell Remoting

Some of you may remember the old WinRM **-quickconfig**, however there is no need for this anymore because of the 

```powershell
Enable-PSRemoting
```
This cmdlet performs all of the actions that WinRM **-quickconfig** does and it also makes some PowerShell specific system tweaks. 

You may also remember the **Set-WSManQuickConfig** cmdlet as well. If you run the help command on this it appears as though it fulfills all requirements for PowerShell remoting. However, **Enable-PSRemoting** invokes that command and makes important session configuration adjustments in order to tightly bind Windows PowerShell to WS-Man and WinRM. 

**Enalbe-PSRemoting** was introduced in Windows PowerShell v4(2013 Windows 8.1). PowerShell remoting is enabled by default in Windows Server 2012 R2, you will need to enable remoting on any Windows client systems and servers running previous versions of Windows Server. 

In administrative PowerShell window run the following command:

```powershell
Enable-PSRemoting
```

![](/Assets/EnablePSRemoting.png)

If you are on Windows Server 2012 R2 or later then you will see that it is already set up and enabled.

You may get an error if one or more of your network adapters is associated with the "Public" location profile. This is because you don't want someone on your hotel's Wi-Fi network attemptinig PowerShell remote connections on your corporate laptop, for example. 

You can force this command to go through by running one of two commands.

```powershell
Enable-PSRemoting -SkipNetworkProfileCheck
```
or

```powershell
Enable-PSRemoting -Force
```

## Creating a Windows PowerShell Remote Session

We use the **Enter-PSSession** cmdlet to establish a remote interactive session with a trusted host. You connect in the security context of your current credentials. If you are a domain administrator, then you'll have thsoe same priveleges on the remote computer. 

We then can use the **Exit-PSSession** to disconnect from the session.

Lets see this in action, on your host machine try the following cmdlets one at a time:

```powershell
hostname
Enter-PSSession -ComputerName SVR1
hostname
Get-EventLog -LogName System -Newest 3
Exit-PSSession
```

![](/Assets/enterSession.png)

I established a remote session from my Windows Server 2012 R2-based server named DC1 to my Server 2012 R2 server named SVR1 without explicitly specifying credentials. 

* I show this by retrieving the local computer's hostname.
* I make the PowerShell remoting session with SVR1 under my current credentials.
* Retreive the hostname to verify we're remotely connected and obatined the newest three entries from that system's event log
* Disconnected fromt the session

Instead of running hostname the second time like above, we can just see that we are indeed connected to the remote session because PowerShell displays the hostname of the remote system.

You can see that we have access to anything up to the limit of our user credentials. This means we have access to all PowerShell modules that are available on the remote system.

```powershell
Get-Module -ListAvailable
```

### **Taking Control of Remote Sessions**

You can create a persistent remoting session, use the **New-PSSession** cmdlet.

```powershell
New-PSSession -ComputerName SVR1
```
![](/Assets/newSession.png)

That session has an ID value, a Name, and is currently open. We can connect to that session by using the cmdlet **Enter-PSSession**

```powershell 
Enter-PSSession -Name "Session2"
```

We can now exit the session and verify that it is gone.

```powershell
Exit-PSSession
Get-PSSession
```
![](/Assets/exitSession.PNG)

We see that the connection still exists. We have to end the session by using the **Remove-PSSession** cmdlet. 

```powershell
Remove-PSSession -Name "Session2"
```

### **Using Variables and Alternate Credentials**

If you want you can always store the PowerShell remote session in a variable for easier access.

```powershell
$svr1 = New-PSSession -ComputerName SVR1 -Credential domain\administrator
```

PowerShell will securely capture your password in a modal dialog box. This way we never have to pass passwords in plain text. 

You can now pipe commands and/or use the dot notation to access items of the variable object.

```powershell
$svr1 | Get-Member

$svr1.id
$svr1.Name
$svr1.ExpiresOn

$svr1 | Remove-PSSession
```

## **Sending Scripts Over the Network**

Sending one single statement over PowerShell remote is easy enough by using the -ComputerName parameter, however you may want to send an entire .ps1 script file. In this case you should use the **Invoke-Command** cmdlet. 

**Invoke-Command** cmdlet is extremely powerful because it leverages WS-Man/WinRM-based remoting, and can target one or more remote hosts simultaneously. 

The two parameters that you'll use most of the time with **Invoke-Command** are -ComputerName and -Scriptblock.

```powershell
Invoke-Command –ComputerName dc1 –ScriptBlock {Get-WMIObject –Class Win32_process | Where-Object {$_.threadcount –gt 8}}
```

We can also combine persistent remote sessions with Invoke-Command

```powershell
$svrone = New-PSSession -ComputerName SVR1
Invoke-Command -ScriptBlock {Get-Module -ListAvailable} -Session $svrone
```

You can even specify specific filepaths with .ps1 scripts. You have to temporarily modify script execution policy on the remote machine before you can send over a script for remote execution.

```powershell
Enter-PSSession -ComputerName SVR1

Set-ExecutionPolicy -ExecutionPolicy Unrestricted
Exit-PSSession

Invoke-Command -ComputerName SVR1 -FilePath "C:\myscript.ps1"
```


<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/Managing_Computers_Remotely/Lab_One_to_One.md" > Continue to Next Topic </a>
