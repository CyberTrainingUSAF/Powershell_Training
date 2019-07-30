# Allow scripts stored on your machine to run unsigned

For security reasons, PowerShell is set up by default to only allow signed scripts to execute. Executing the following
command will allow you to run unsigned scripts (you must run PowerShell as Administrator to do this).
```powershell
Set-ExecutionPolicy RemoteSigned
```
Another way to run PowerShell scripts is to use **Bypass** as **ExecutionPolicy**:
```powershell
powershell.exe -ExecutionPolicy Bypass -File "c:\MyScript.ps1"
```

Or from within your existing PowerShell console or ISE session by running:
```powershell
Set-ExecutionPolicy Bypass Process
```
A temporary workaround for execution policy can also be achieved by running the PowerShell executable and
passing any valid policy as **-ExecutionPolicy parameter**. The policy is in effect only during process' lifetime, so no
administrative access to the registry is needed.
```powershell
C:\>powershell -ExecutionPolicy RemoteSigned
```
There are multiple other policies available, and sites online often encourage you to use **Set-ExecutionPolicy
Unrestricted**. This policy stays in place until changed, and lowers the system security stance. This is not advisable.
Use of **RemoteSigned** is recommended because it allows locally stored and written code, and requires remotely
acquired code be signed with a certificate from a trusted root.
Also, beware that the Execution Policy may be enforced by Group Policy, so that even if the policy is changed to
**Unrestricted** system-wide, Group Policy may revert that setting at its next enforcement interval (typically 15
minutes). You can see the execution policy set at the various scopes using **Get-ExecutionPolicy -List**

# Simple Scripting Lab

In your favorite editor:
Type the following commands( Change the "Out-File" to a location of your choosing)
```powershell
"Hello, World!" | Out-File C:\ps\test.txt
```
Save the File with a ".ps1" extension.

Now on powershell navigate to the location of your file and execute the script by typing "./nameofile.extension"


