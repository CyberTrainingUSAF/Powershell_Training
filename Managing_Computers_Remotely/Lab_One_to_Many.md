In this example, I’m running Windows ISE on my DC1 server and making remote connections to my two member servers, MEM1 and MEM2. If you are fortunate enough to have a practice lab set up with three virtual machines (VMs), you can follow these steps to see remoting in action yourself:

1. First of all, make sure you’ve started ISE with administrative credentials. (Hint: If the title bar doesn’t say Administrator, you’re not in an elevated session.)

2. If the Commands add-on is displayed, which it usually is by default, click its close button to remove it.

3. Click File > New Remote PowerShell Tab and establish an administrative session on another computer in your environment.

4. Once you see the shaded remote tab, click the Show Script Pane Top button to place a blank .ps1 script pane above the “live” console session below.

5. Now we’re cooking with gas! The goal of the script we’re about to create is to generate a text-based report that lists the Windows roles and features that are currently installed on the local system.

To get started, create a folder named scripts in the root of your local server’s C: drive. We’ll use this folder as a central repository for reports generated on remote machines.

Add the following lines to your new script file; make sure that you’re doing this in the remote tab!
```powershell
Import-Module –Name ServerManager
Get-WindowsFeature | Where-Object {$_.Installed –eq "True"}
```
If you’re running Windows PowerShell v3 or later, you technically don’t need the Import-Module statement. However, I included it for educational purposes.

6. Now let’s modify that Get-WindowsFeature pipeline to generate a concise list of installed roles and features on the remote computer:

```powershell
Get-WindowsFeature | Where-Object {$_.Installed –eq "True"}| Format-List –Property Name, DisplayName, Subfeatures –Expand Both
```
Incidentally, I threw in the –Expand Both parameter and value to ensure that the subfeature hash table displayed all of its contents instead of truncating like it normally does.

7. We’re almost finished with our script. Let’s make the pipeline even longer by making the output filename pick up the hostname of the local computer and copying the file to the scripts folder on our dc1 server. I’m just going to give you the entire script here at once:

```powershell
Import-Module –Name ServerManager
$hostname = Hostname
Get-WindowsFeature | Where-Object {$_.Installed -eq "True"} | Sort-Object -Property Name | Format-List -Property Name, DisplayName, Subfeatures -Expand Both | Out-File "c:\$hostname.txt"
Copy-Item -Path "c:\$hostname.txt" -Destination "\\dc1\c$\scripts"
```
Tip

If you receive an “Access is denied” error when performing the file copy, you may need to adjust any NTFS or shared folder permissions as necessary and retry. You might need to close and reopen the remote session in order to refresh the security account token associated with the session as well.

8. Before you close the remote tab, click File > Save As to save your new script file. Put it in your local system’s scripts folder (you should see the report from the remote machine in here as well), and name it remote-features.ps1.

9. Close the remote tab. Remember, you should be at your source computer—in my case, the dc1 box.

10. From the local session, either in the ISE or from a PowerShell console session, use one-to-many remoting to blast the script at any other remoting-enabled hosts in your network:

```powershell
$computers = New-PSSession –ComputerName dc1, mem2

Invoke-Command –Session $computers –FilePath "c:\scripts\remote-features.ps1"
```
If you receive errors, closely scrutinize the first few lines of the error message and debug accordingly. Welcome to the wild and wooly world of administrative scripting.
