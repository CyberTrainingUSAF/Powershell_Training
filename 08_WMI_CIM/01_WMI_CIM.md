# Windows Management Instrumentation
Windows Management Instrumentation (WMI) was introduced as a downloadable component with Windows 95 and NT. Windows 2000 had WMI pre-installed, and it has since become a core part of the operating system.

WMI can be used to access a huge amount of information about the computer system. This includes printers, device drivers, user accounts, ODBC, and so on; there are hundreds of classes to explore.

In this chapter, we will be covering the following topics:

Working with WMI
CIM cmdlets
WMI cmdlets
Permissions

## Working with WMI
The scope of WMI is vast, which makes it a fantastic resource for automating processes. WMI classes are not limited to the core operating system; it is not uncommon to find classes created after software or device drivers have been installed.

Given the scope of WMI, finding an appropriate class can be difficult. PowerShell itself is well equipped to explore the available classes.

## WMI classes
PowerShell, as a shell for working with objects, presents WMI classes in a very similar manner to .NET classes or any other object. There are a number of parallels between WMI classes and .NET classes.

A WMI class is used as the recipe to create an instance of a WMI object. The WMI class defines properties and methods. The WMI class Win32_Process is used to gather information about running processes in a similar manner to the Get-Process command.

The Win32_Process class has properties such as ProcessId, Name, and CommandLine. It has a terminate method that can be used to kill a process, as well as a create static method that can be used to spawn a new process.

WMI classes reside within a WMI namespace. The default namespace is root\cimv2; classes such as Win32_OperatingSystem and Win32_LogicalDisk reside in this namespace.

## WMI commands
PowerShell has two different sets of commands dedicated to working with WMI.

The CIM cmdlets were introduced with PowerShell 3.0. They are compatible with the Distributed Management Task Force (DMTF) standard DSP0004. A move towards compliance with open standards is critical as the Microsoft world becomes more diverse.

WMI itself is a proprietary implementation of the CIM server, using the Distributed Component Object Model (DCOM) API to communicate between the client and server.

Standards compliance and differences in approach aside, there are solid, practical reasons to consider when choosing which one to use.

Some properties of CIM cmdlets are as follows:

They are available in both Windows PowerShell and PowerShell Core.
They handle date conversion natively.
They have a flexible approach to networking. They use WSMAN for remote connections by default, but can be configured to use DCOM over RPC.
Some properties of WMI cmdlets are as follows:

They are only available in Windows PowerShell, not in PowerShell Core
They do not automatically convert dates
They use DCOM over RPC exclusively
They can be used for all WMI operations
They have been superseded by the CIM cmdlets

## The WMI Query Language
Before diving into the individual commands, it will help to have a grasp of the query language used for WMI queries. The query language is useful when querying classes that return multiple values.

The WMI Query Language (WQL) is used to build queries in WMI for both the CIM and WMI commands.

WQL implements a subset of Structured Query Language (SQL). The keywords that we will look at are traditionally written in uppercase; however, WMI queries are not case-sensitive.

Both the CIM and WMI cmdlets support Filter and Query parameters, which accept WQL queries.
## Understanding SELECT, WHERE, and FROM
The SELECT, WHERE, and FROM keywords are used with the Query parameter.

The generalized syntax for the Query parameter is as follows:

SELECT <Properties> FROM <WMI Class> 
SELECT <Properties> FROM <WMI Class> WHERE <Condition> 
The wildcard, *, may be used to request all available properties or a list of known properties may be requested:

Get-CimInstance -Query "SELECT * FROM Win32_Process" 
Get-CimInstance -Query "SELECT ProcessID, CommandLine FROM Win32_Process" 
The WHERE keyword is used to filter results returned by SELECT; for example, see the following:
```
Get-CimInstance -Query "SELECT * FROM Win32_Process WHERE ProcessID=$PID" 
```
WQL and arrays
WQL cannot filter array-based properties (for example, the capabilities property of Win32_DiskDrive).

## Escape sequences and wildcard characters
The backslash character, \, is used to escape the meaning of characters in a WMI query. This might be used to escape a wildcard character, quotes, or itself. For example, the following WMI query uses a path; each instance of \ in the path must be escaped:
```
Get-CimInstance Win32_Process -Filter "ExecutablePath='C:\\Windows\\Explorer.exe'" 
```
About Win32_Process and the Path property

The Path property is added to the output from the Win32_Process class by PowerShell. While it appears in the output, the property cannot be used to define a filter, nor can Path be selected using the Property parameter of either Get-CimInstance or Get-WmiObject.
```
Get-Member shows that it is a ScriptProperty as follows:
Get-CimInstance Win32_Process -Filter "ProcessId=$pid" | Get-Member -Name Path
Get-WmiObject Win32_Process -Filter "ProcessId=$pid" | Get-Member -Name Path
```
WQL defines two wildcard characters that can be used with string queries:

The % (percentage) character matches any number of characters and is equivalent to using * in a filesystem path or with the -like operator
The _ (underscore) character matches a single character and is equivalent to using ? in a filesystem path or with the -like operator
The following query filters the results of Win32_Service, including services with paths starting with a single drive letter and ending with .exe:
```
Get-CimInstance Win32_Service -Filter 'PathName LIKE "_:\\%.exe"' 
```

## Logic operators
Logic operators may be used with the Filter and Query parameters.

The examples in the following table are based on the following command:
```
Get-CimInstance Win32_Process -Filter "<Filter>"  

```

![image](https://user-images.githubusercontent.com/47218880/63103589-0343cd80-bf43-11e9-816f-ceeac76b368e.png)

## Comparison operators
Comparison operators may be used with the Filter and Query parameters.

The examples in the following table are based on the following command:
```
Get-CimInstance Win32_Process -Filter "<Filter>"  

```
![image](https://user-images.githubusercontent.com/47218880/63103701-37b78980-bf43-11e9-8a34-c31d048a4620.png)

## Quoting values
When building a WQL query, string values must be quoted; numeric and Boolean values do not need quotes.

As the filter is also a string, this often means nesting quotes within one another. The following techniques may be used to avoid needing to use PowerShell's escape character.

For filters or queries containing fixed string values, use either of the following styles. Use single quotes outside and double quotes inside:
```
Get-CimInstance Win32_Process -Filter 'Name="powershell.exe"' 
```
Alternatively, use double quotes outside and single quotes inside:
```
Get-CimInstance Win32_Process -Filter "Name='powershell.exe'" 
```
For filters or queries containing PowerShell variables or sub-expressions, use double quotes outside, as variables within a single-quoted string that will not expand:
```
Get-CimInstance Win32_Process -Filter "ProcessId=$PID" 
Get-CimInstance Win32_Process -Filter "ExecutablePath LIKE '$($pshome -replace '\\', '\\')%'" 
```
Regex recap

The regular expression '\\' represents a single literal '\', as the backslash is normally the escape character. Each '\' in the pshome path is replaced with '\\' to account for WQL using '\' as an escape character as well.
Finally, if a filter contains several conditions, consider using the format operator, as shown in this splatting block:
```
$params = @{
    ClassName = 'Win32_Process'
    Filter    = 'ExecutablePath LIKE "{0}%" AND WorkingSetSize<{1}' -f 
        ($env:WINDIR -replace '\\', '\\'), 
        100MB
}
Get-CimInstance @params
```
## Associated classes
WMI classes often have several different associated or related classes; for example, each instance of Win32_Process has an associated class, CIM_DataFile.

Associations between two classes are expressed by a third class. In the case of Win32_Process and CIM_DataFile, the relationship is expressed by the CIM_ProcessExecutable class.

The relationship is defined by using the antecedent and dependent properties, as shown in the following example:
```
PS> Get-CimInstance CIM_ProcessExecutable |
>>     Where-Object Dependent -match $PID |
>>     Select-Object -First 1


Antecedent         : CIM_DataFile (Name = "C:\WINDOWS\System32\WindowsPowerShell\v...)
Dependent          : Win32_Process (Handle = "11672")
BaseAddress        : 2340462460928
GlobalProcessCount : 
ModuleInstance     : 4000251904
ProcessCount       : 0
PSComputerName     : 
```
This CIM_ProcessExecutable class does not need to be used directly.

## WMI object paths
A WMI path is required to find classes associated with an instance. The WMI object path uniquely identifies a specific instance of a WMI class.

The object path is made up of a number of components:
```
<Namespace>:<ClassName>.<KeyName>=<Value> 
```
The namespace can be omitted if the class is under the default namespace, root\cimv2.

The KeyName for a given WMI class can be discovered in a number of ways. In the case of Win32_Process, the key name might be discovered by using any of the following methods.

It can be discovered by using the CIM cmdlets:
```
(Get-CimClass Win32_Process).CimClassProperties | 
    Where-Object { $_.Flags -band 'Key' } 
```
It can be discovered by using the MSDN website, which provides the descriptions of each property (and method) exposed by the class: https://msdn.microsoft.com/en-us/library/aa394372(v=vs.85).aspx.

Having identified a key, only the value remains to be found. In the case of Win32_Process, the key (handle) has the same value as the process ID. The object path for the Win32_Process instance associated with a running PowerShell console is, therefore, the following:
```
root\cimv2:Win32_Process.Handle=$PID 
```
The namespace does not need to be included if it uses the default, root\cimv2; the object path can be shortened to the following:
```
Win32_Process.Handle=$PID 
```
Get-CimInstance and Get-WmiObject will not retrieve an instance from an object path, but the Wmi type accelerator can:
```
PS> [Wmi]"Win32_Process.Handle=$PID" | Select-Object Name, Handle

Name               Handle
----               ------
powershell_ise.exe 13020 


```

## Using ASSOCIATORS OF
The ASSOCIATORS OF query may be used for any given object path; for example, using the preceding object path results in the following command:
```
Get-CimInstance -Query "ASSOCIATORS OF {Win32_Process.Handle=$PID}" 
```
This query will return objects from three different classes: Win32_LogonSession, Win32_ComputerSystem, and CIM_DataFile. The classes returned are shown in the following example:
```
PS> Get-CimInstance -Query "ASSOCIATORS OF {Win32_Process.Handle=$PID}"  |
>>     Select-Object CimClass -Unique

CimClass
--------
root/cimv2:Win32_ComputerSystem
root/cimv2:Win32_LogonSession
root/cimv2:CIM_DataFile
```
The query can be refined to filter a specific resulting class; an example is as follows:
```
Get-CimInstance -Query "ASSOCIATORS OF {Win32_Process.Handle=$PID} WHERE ResultClass = CIM_DATAFILE" 
```
The value in the ResultClass condition is deliberately not quoted.
The result of this operation is a long list of files that are used by the PowerShell process. A snippet of this is shown as follows:
```
PS> Get-CimInstance -Query "ASSOCIATORS OF {Win32_Process.Handle=$PID} WHERE ResultClass = CIM_DATAFILE" | 
>>     Select-Object Name

Name 
---- 
c:\windows\system32\windowspowershell\v1.0\powershell_ise.exe 
c:\windows\system32\ntdll.dll 
c:\windows\system32\mscoree.dll 
c:\windows\system32\sysfer.dll 
c:\windows\system32\kernel32.dll 
```
## CIM cmdlets
The Common Information Model (CIM) commands are as follows:
```
Get-CimAssociatedInstance
Get-CimClass
Get-CimInstance
Get-CimSession
Invoke-CimMethod
New-CimInstance
New-CimSession
New-CimSessionOption
Register-CimIndicationEvent
Remove-CimInstance
Remove-CimSession
Set-CimInstance
```
Each of the CIM cmdlets uses either the ComputerName or CimSession parameter to target the operation at another computer.

## Getting instances
The Get-CimInstance command is used to execute queries for instances of WMI objects, an example is as follows:
```
Get-CimInstance -ClassName Win32_OperatingSystem 
Get-CimInstance -ClassName Win32_Service 
Get-CimInstance -ClassName Win32_Share 
```
A number of different parameters are available when using Get-CimInstance. The command can be used with a filter, as follows:
```
Get-CimInstance Win32_Directory -Filter "Name='C:\\Windows'" 
Get-CimInstance CIM_DataFile -Filter "Name='C:\\Windows\\System32\\cmd.exe'" 
Get-CimInstance Win32_Service -Filter "State='Running'" 
```
When returning large amounts of information, the Property parameter can be used to reduce the number of fields returned by a query:
```
Get-CimInstance Win32_UserAccount -Property Name, SID 
```
The Query parameter can also be used, although it is rare to find a use for this that cannot be served by the individual parameters:
```
Get-CimInstance -Query "SELECT * FROM Win32_Process" 
Get-CimInstance -Query "SELECT Name, SID FROM Win32_UserAccount" 
```

## Getting classes
The Get-CimClass command is used to return an instance of a WMI class:
```
PS> Get-CimClass Win32_Process

NameSpace: ROOT/cimv2

CimClassName     CimClassMethods                CimClassProperties 
------------     ---------------                ------------------ 
Win32_Process    {Create, Terminate, Get...}    {Caption, Description, InstallDate, Name...} 
```
The Class object describes the capabilities of that class. By default, Get-CimClass lists classes from the root\cimv2 namespace.

The Namespace parameter will fill using tab completion; that is, if the following partial command is entered, pressing Tab repeatedly will cycle through the possible root namespaces:
```
Get-CimClass -Namespace <tab, tab, tab> 
```
The child namespaces of a given namespace are listed in a __Namespace class instance. For example, the following command returns the namespaces under root:
```
Get-CimInstance __Namespace -Namespace root 
```
Extending this technique, it is possible to recursively query __Namespace to find all of the possible namespace values. Certain WMI namespaces are only available to administrative users (run as administrator); the following function may display errors for some namespaces:
```
function Get-CimNamespace { 
    param ( 
        $Namespace = 'root' 
    ) 
       
    Get-CimInstance __Namespace -Namespace $Namespace | ForEach-Object { 
        $childNamespace = Join-Path $Namespace $_.Name 
        $childNamespace 
 
        Get-CimNamespace -Namespace $childNamespace 
    } 
} 
Get-CimNamespace 

```
## Calling methods
The Invoke-CimMethod command may be used to call a method. The CIM class can be used to find details of the methods that a class supports:
```
PS> (Get-CimClass Win32_Process).CimClassMethods 

Name ReturnType Parameters Qualifiers 
---- ---------- ---------- ---------- 
Create UInt32 {CommandLine...} {Constructor...}
Terminate UInt32 {Reason} {Destructor...} 
GetOwner UInt32 {Domain...} {Implemented...}
GetOwnerSid UInt32 {Sid} {Implemented...}
```
The method with the Constructor qualifier can be used to create a new instance of Win32_Process.

The Parameters property of a specific method can be explored to find out how to use a method:
```
PS> (Get-CimClass Win32_Process).CimClassMethods['Create'].Parameters

Name                          CimType    Qualifiers
----                          -------    ----------
CommandLine                    String    {ID, In, MappingStrings}
CurrentDirectory               String    {ID, In, MappingStrings}
ProcessStartupInformation    Instance    {EmbeddedInstance, ID, In, MappingStrings}
ProcessId                      UInt32    {ID, MappingStrings, Out}
```
If an argument has the In qualifier, it can be passed in when creating an object. If an argument has the Out qualifier, it will be returned after the instance has been created. Arguments are passed in using a hashtable.

When creating a process, the CommandLine argument is required; the rest can be ignored until later:
```
$params = @{
    ClassName  = 'Win32_Process'
    MethodName = 'Create'
    Arguments  = @{
        CommandLine = 'notepad.exe' 
    } 
}
$return = Invoke-CimMethod @params
```
The return object holds three properties in the case of Win32_Process, as follows:
```
PS> $return

ProcessId    ReturnValue     PSComputerName
---------    -----------     --------------
    15172              0 
```
PSComputerName is blank when the request is local. The ProcessId is the Out property listed under the method parameters. ReturnValue indicates whether or not the operation succeeded, and 0 indicates that it was successful.

A nonzero value indicates that something went wrong, but the values are not translated in PowerShell. The return values are documented on MSDN at https://msdn.microsoft.com/en-us/library/aa389388(v=vs.85).aspx.

The Create method used here creates a new instance. The other methods for Win32_Process act against an existing instance (an existing process).

Extending the preceding example, a process can be created and then terminated:
```
$params = @{
    ClassName  = 'Win32_Process'
    MethodName = 'Create'
    Arguments  = @{ 
        CommandLine = 'notepad.exe' 
    }
}
$return = Invoke-CimMethod @params

pause

Get-CimInstance Win32_Process -Filter "ProcessID=$($return.ProcessId)" | 
    Invoke-CimMethod -MethodName Terminate 
```
The pause command will wait for return to be pressed before continuing; this gives us the opportunity to show that Notepad was opened before it is terminated.

The Terminate method has an optional argument that is used as the exit code for the terminate process. This argument may be added using hashtable; in this case, a (made up) value of 5 is set as the exit code:
```
$invokeParams = @{
    ClassName  = 'Win32_Process'
    MethodName = 'Create'
    Arguments  = @{
        CommandLine = 'notepad.exe' 
    }
}
$return = Invoke-CimMethod @invokeParams

$getParams = @{
    ClassName = 'Win32_Process'
    Filter    = 'ProcessId={0}' -f $return.ProcessId
}
Get-CimInstance @getParams |
    Invoke-CimMethod -MethodName Terminate -Arguments @{Reason = 5}
```
Invoke-CimMethod returns an object with a ReturnValue. A return value of 0 indicates that the command succeeded. A nonzero value indicates an error condition. The meaning of the value will depend on the WMI class.

The return values associated with the Terminate method of Win32_Process are documented on MSDN at https://msdn.microsoft.com/en-us/library/aa393907(v=vs.85).aspx.

## Creating instances
The arguments for Win32_Process, create include a ProcessStartupInformation parameter. ProcessStartupInformation is described by a WMI class, Win32_ProcessStartup.

There are no existing instances of Win32_ProcessStartup (Get-CimInstance), and the class does not have a Create method (or any other constructor).

New-CimInstance can be used to create a class:
```
$class = Get-CimClass Win32_ProcessStartup 
$startupInfo = New-CimInstance -CimClass $class -ClientOnly 
```
New-Object can also be used:
```
$class = Get-CimClass Win32_ProcessStartup 
$startupInfo = New-Object CimInstance $class 
```
Finally, the new method may be used:
```
$class = Get-CimClass Win32_ProcessStartup 
$startupInfo = [CimInstance]::new($class)
```
Properties may be set on the created instance; the effect of each property is documented on MSDN at https://msdn.microsoft.com/en-us/library/aa394375(v=vs.85).aspx.

In the following example, properties are set to dictate the position and title of a cmd.exe window:
```
$class = Get-CimClass Win32_ProcessStartup 
$startupInfo = New-CimInstance -CimClass $class -ClientOnly 
$startupInfo.X = 50 
$startupInfo.Y = 50 
$startupInfo.Title = 'This is the window title' 

$params = @{
    ClassName  = 'Win32_Process'
    MethodName = 'Create'
    Arguments  = @{ 
        CommandLine               = 'cmd.exe' 
        ProcessStartupInformation = $startupInfo 
    }
} 
$returnObject = Invoke-CimMethod @params 
```

## Working with CIM sessions
As mentioned earlier in this chapter, a key feature of the CIM cmdlets is their ability to change how connections are formed and used.

The Get-CimInstance command has a ComputerName parameter, and when this is used, the command automatically creates a session to a remote system using WSMAN. The connection is destroyed as soon as the command completes.

While Get-CimInstance supports basic remote connections, it does not provide a means of authenticating a connection, nor can the protocol be changed.

The Get-CimSession, New-CimSession, New-CimSessionOption, and Remove-CimSession commands are optional commands that can be used to define the behavior of remote connections.

The New-CimSession command creates a connection to a remote server an example is as follows:
```
PS> $cimSession = New-CimSession -ComputerName Remote1
PS> $cimSession

Id           : 1
Name         : CimSession1
InstanceId   : 1cc2a889-b649-418c-94a2-f24e033883b4
ComputerName : Remote1
Protocol     : WSMAN 
```
Alongside the other parameters, New-CimSession has a Credential parameter that can be used in conjunction with Get-Credential to authenticate a connection.

If the remote system does not, for any reason, present access to WSMAN, it is possible to switch the protocol down to DCOM by using the New-CimSessionOption command:
```
PS> $option = New-CimSessionOption -Protocol DCOM
PS> $cimSession = New-CimSession -ComputerName Remote1 –SessionOption $option
PS> $cimSession

Id           : 2
Name         : CimSession2
InstanceId   : 62b2cb56-ec84-472c-a992-4bee59ee0618
ComputerName : Remote1
Protocol     : DCOM 
```
The New-CimSessionOption command is not limited to protocol switching; it can affect many of the other properties of the connection, as shown in the help and the examples for the command.

Once a session has been created, it exists in the memory until it is removed. The Get-CimSession command shows a list of connections that have been formed, and the Remove-CimSession command permanently removes connections.

## Associated classes
The Get-CimAssociatedClass command replaces the use of the ASSOCIATORS OF query type when using the CIM cmdlets.

The following command gets the class instances associated with Win32_NetworkAdapterConfiguration. As the arguments for the Get-CimInstance command are long strings, splatting is used to pass the parameters into the command:
```
$params = @{ 
    ClassName = 'Win32_NetworkAdapterConfiguration' 
    Filter    = 'IPEnabled=TRUE AND DHCPEnabled=TRUE' 
} 
Get-CimInstance @params | Get-CimAssociatedInstance 
```
The following example uses Get-CimAssociatedClass to get the physical interface associated with the IP configuration:
```
$params = @{ 
    ClassName = 'Win32_NetworkAdapterConfiguration' 
    Filter    = 'IPEnabled=TRUE AND DHCPEnabled=TRUE' 
} 
Get-CimInstance @params | ForEach-Object { 
    $adapter = $_ | Get-CimAssociatedInstance -ResultClassName Win32_NetworkAdapter 
 
    [PSCustomObject]@{ 
        NetConnectionID = $adapter.NetConnectionID 
        Speed           = [Math]::Round($adapter.Speed / 1MB, 2) 
        IPAddress       = $_.IPAddress 
        IPSubnet        = $_.IPSubnet 
        Index           = $_.Index 
        Gateway         = $_.DefaultIPGateway 
    } 
} 
```
## The WMI cmdlets
The WMI cmdlets have been superseded by the CIM cmdlets. The WMI cmdlets are not available in PowerShell Core, but the type accelerators are.
The WMI commands are as follows:
```
Get-WmiObject
Invoke-WmiMethod
Register-WmiEvent
Remove-WmiObject
Set-WmiInstance
```
In addition to the commands, three type accelerators are available:
```
[Wmi]: System.Management.ManagementObject
[WmiClass]: System.Management.ManagementClass
[WmiSearcher]: System.Management.ManagementObjectSearcher
```
Each of the WMI cmdlets uses the ComputerName parameter to aim the operation at another computer. The WMI cmdlets also support a credential parameter and other authentication options affecting the authentication method.

Both the Wmi and WmiClass type accelerators can be written to use a remote computer by including the computer name an example is as follows:
```
[Wmi]"\\RemoteComputer\root\cimv2:Win32_Process.Handle=$PID" 
[WmiClass]"\\RemoteComputer\root\cimv2:Win32_Process" 
```
## Getting instances
The Get-WmiObject command is used to execute queries for instances of WMI objects an example is as follows:
```
Get-WmiObject -Class Win32_ComputerSystem 
```
The type accelerator, WmiSearcher, may also be used to execute queries:
```
([WmiSearcher]"SELECT * FROM Win32_Process").Get() 
```
## Getting classes
The Get-WmiObject command is used to get classes:
```
Get-WmiObject Win32_Process -List 
```
The WMI cmdlets are able to recursively list classes in namespaces. The following command lists the classes in root\cimv2 and any child namespaces:
```
Get-WmiObject -List -Recurse
```
In addition to the list parameter, the WmiClass type accelerator can be used:
```
[WmiClass]"Win32_Process"   
```
## Calling methods
Calling a method on an existing instance of an object found using Get-WmiObject is similar to any .NET method call.

The following example gets and restarts the DNS Client service. The following operation requires administrative access:
```
$service = Get-WmiObject Win32_Service -Filter "DisplayName='DNS Client'" 
$service.StopService()     # Call the StopService method 
$service.StartService()    # Call the StartService method  
```
The WMI class can be used to find the details of a method; for example, the Create method of Win32_Share, as follows:
```
PS> (Get-WmiObject Win32_Share -List).Methods['Create']

Name          : Create
InParameters  : System.Management.ManagementBaseObject
OutParameters : System.Management.ManagementBaseObject
Origin        : Win32_Share
Qualifiers    : {Constructor, Implemented, MappingStrings, Static}
```
Where the Invoke-CimMethod command accepts a hashtable, the Invoke-WmiMethod command expects arguments to be passed as an array, in a specific order. The order can be retrieved by using the GetMethodParameters method of the WMI class:
```
PS> (Get-WmiObject Win32_Share -List).GetMethodParameters('Create')


__GENUS          : 2
__CLASS          : __PARAMETERS
__SUPERCLASS     : 
__DYNASTY        : __PARAMETERS
__RELPATH        : 
__PROPERTY_COUNT : 7
__DERIVATION     : {}
__SERVER         : 
__NAMESPACE      : 
__PATH           : 
Access           : 
Description      : 
MaximumAllowed   : 
Name             : 
Password         : 
Path             : 
Type             : 
PSComputerName   :
```
To create a share, the argument list must therefore contain an argument for Access, then Description, then MaximumAllowed, and so on. If the argument is optional, it can be set to null; however, PowerShell is unable to say which are mandatory, so a trip to MSDN is required: https://msdn.microsoft.com/en-us/library/aa389393(v=vs.85).aspx.

Having established that Path, Name, and Type are mandatory; an array of arguments can be created in the order described by GetMethodParameters:
```
$argumentList = $null,            # Access 
                $null,            # Description 
                $null,            # MaximumAllowed 
                'Share1',         # Name 
                $null,            # Password 
                'C:\Temp\Share1', # Path 
                0                 # Type (Disk Drive) 
Invoke-WmiMethod Win32_Share -Name Create -ArgumentList $argumentList 
```
The return value describes the result of the operation; a ReturnValue of 0 indicates success. As this operation requires administrator privileges (run as administrator), a return value of 2 is used to indicate that it was run without sufficient rights.

If the folder used in the previous example does not exist, the ReturnValue will be set to 24.

Adding the ComputerName parameter to Invoke-WmiMethod will create a share on a remote machine.

Arrays of null values are messy

This method of supplying arguments to execute a method is difficult to work with for all but the simplest of methods. An alternative is to use the .NET method InvokeMethod on the class object:
```
$class = Get-WmiObject Win32_Share -List
$inParams = $class.GetMethodParameters('Create')
$inParams.Name = 'Share1'
$inParams.Path = 'C:\Temp\Share1'
$inParams.Type = 0
$return = $class.InvokeMethod('Create', $inParams, $null)
```
The last argument, set to null here, is InvokeMethodOptions, which is most often used to define a timeout for the operation. Doing so is beyond the scope of this chapter.

To create a share on a remote computer, use the ComputerName parameter with Get-WmiObject.

## Creating instances
An instance of a WMI class can be created using the CreateInstance method of the class. The following example creates an instance of Win32_Trustee:
```
(Get-WmiObject Win32_Trustee -List).CreateInstance() 
```
