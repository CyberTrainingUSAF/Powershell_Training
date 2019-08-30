
## **Everything in PowerShell is an Object (In Progress)**

Everything in PowerShell is an object. We know this already.

As a reminder we can see this by looking at a simple string variable.
```powershell
$string = "hello world"
```
We can run the length parameter on this object
```powershell
$string.length
```
To show even further, we can pipe this variable to the Get-Member cmdlet and see all of the methods that we can apply to this string object. 
```powershell
$string | Get-Member
```
Lets get into PowerShell custom objects and how we can build them.

There is the older **Get-Member** approach, and then there is the newer approach of using hashtables and the PSCustomObject accelerator.

```powershell
#Get information about our Windows Installation.
$OS = Get-CimInstance -ClassName Win32_OperatingSystem
#Get information about the system's BIOS
$BIOS = Get-CimInstance -ClassName Win32_BIOS

$OS | Format-List
$BIOS | Format-List

#Create a customer object and add members to it.
$CustomObject1 = New-Object -TypeName PSCustomObject
$CustomObject1 |
Add-Member -MemberType NoteProperty -Name 'OS Version' -Value $OS Version -PassThru |
Add-Member -MemberType NoteProperty -Name 'Install Date' -Value $OS InstallDate -PassThru |
Add-Member -MemberType NoteProperty -Name 'BIOS Manufacturer' -Value $BIOS Manufacturer
$CustomObject1 Format-List
```

```powershell
#Create a customer object using the [PSCustomObject] type accelerator on a hashtable
$CustomObject2 = [PSCustomObject]@{
'OS Version = $OS.Version
'Install Date' = $OS.InstallDate
'BIOS Manufacturer = $BIOS.Manufacturer
}
```
* Note that after you create the original hash table that you can still use the **Add-Member** cmdlet like above to add to it. 

To remove a property off an object you can do the following
```powershell
$CustomObject2.psobject.properties.remove('Install Date')
```
$CustomObject2 | Format-List

So we can see that there are many things that we can do with custom objects
```powershell
#Display the object in a table
$CustomObject2 | Format-Table
```
[insert image]
```powershell
#Display the object in a list
$CustomObject2 | Format-List
```
[insert image]
```powershell
#Convert the object to JSON format
$CustomObject2 | ConvertTo-Json
```
[insert image]
```powershell
#Convert the object to XML format
$CustomObject2 | ConverTo-Xml
```
[insert image]
```powershell
#Convert the object to HTML format
$CustomObject2 | ConverTo-Html
```
[insert image]
```powershell
#Format the object in hex
$CustomObject2 | Out-String | Format-Hex
```
[insert image]

You can also create a PSCustomObject from a hashtable that is already made.

```powershell
$myHashTable = @{
Name = 'Daniel'
Age = '30'
Gender = 'Male'
PowerLevel = 'Over 9000'
}

$myCustomObject = [PSCustomObject]$myHashTable
```
For earlier versions of PowerShell you will want to do the following if you have a hash table already set up
```powershell
$myCustomObject = New-Object -TypeName PSObject -Property $myHashTable
```
```powershell
#Looping to set up a PSCustomObject

$Path = "c:\scripts"
$Directory = Get-Acl -Path $Path 
ForEach ($Dir in $Directory.Access){ 
    [PSCustomObject]@{ 
        Path = $Path 
        Owner = $Directory.Owner 
        Group = $Dir.IdentityReference 
        AccessType = $Dir.AccessControlType 
        Rights = $Dir.FileSystemRights 
    }#EndPSCustomObject
}#EndForEach  
```
