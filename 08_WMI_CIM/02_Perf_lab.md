## Lab
```
Note

For this lab, you need any Windows computer running PowerShell v3 or later.
```
Take some time to complete the following hands-on tasks. Much of the difficulty in using WMI is in finding the class that will give you the information you need, so much of your time in this lab will be spent tracking down the right class. Try to think in keywords (we’ll provide some hints), and use a WMI explorer to quickly search through classes (the WMI Explorer we use lists classes alphabetically, making it easier for us to validate our guesses). Keep in mind that PowerShell’s help system can’t help you find WMI classes.


1.  What class can you use to view the current IP address of a network adapter? Does the class have any methods that you could use to release a DHCP lease? (Hint: network is a good keyword here.)

2.  Create a table that shows a computer name, operating system build number, operating system description (caption), and BIOS serial number. (Hint: you’ve seen this technique, but you need to reverse it to query the OS class first, and then query the BIOS second.)

3.  Query a list of hotfixes using WMI. (Hint: Microsoft formally refers to these as quick-fix engineering.) Is the list different from that returned by the Get-Hotfix cmdlet?

4.  Display a list of services, including their current statuses, their start modes, and the accounts they use to log on.

5.  Using the CIM cmdlets, list all available classes in the SecurityCenter2 namespace with Product as part of the name.

6.  Once you discover the name, use the CIM cmdlets to display any antispyware application. You can also check for antivirus products.





