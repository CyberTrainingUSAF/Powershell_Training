<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---
## Working with Files


**File I/O**

So far, all the input to commands has been in the form of parameters, and the output has been either to the console (with write-host), or to the output stream. In this chapter, we will look at some of the ways that PowerShell gives us to work with files. The topics that will be covered include the following:

* Reading and writing text files
* Working with CSV files

**Reading and writing text files**

One method of reading a text file in PowerShell is to use the Get-Content cmdlet. Get-Content outputs the contents of the file as a collection of strings. If we have a text file called Servers.txt that contains the names of the SQL Servers in our environment, we would use the following code to read the file and get the status of the MSSQLSERVER service on the servers. Note that with this code, the file will not contain anything but the server names, each on a separate line with no blank lines:

![image](https://user-images.githubusercontent.com/47218880/61823512-934d9600-ae21-11e9-8fa1-9e4c522b97c9.png)

If you don't want the file to be split into lines, there is a –Raw switch parameter that causes the cmdlet to output the entire file as a single string. As an example, if we have a text file with three lines, we can see the difference between when we use the default operation and when we use –Raw by counting the number of strings returned:

![image](https://user-images.githubusercontent.com/47218880/61823574-ab251a00-ae21-11e9-8eec-d9533bc9918c.png)

If you want only the beginning or the end of the file, you could use Select-Object with the –First or –Last parameters, but it turns out that Get-Content has this covered as well. To get the beginning of the file, you can use the –TotalCount parameter to specify how many lines to get. To get the end of the file, use the –Tail parameter, instead of the –TotalCount parameter, which lets you specify how many of the end lines to output.

**TIP**

> Reminder!

Always filter the pipeline as early as possible. Although, you could achieve the same thing with Select-Object as you can with the –TotalCount and –Tail parameters, filtering at the source (Get-Content) will be faster because less objects will have to be created and passed on to the pipeline.

**Writing text files**

There are several ways to write to a text file. First, if you have the entire contents of the file in a variable or as the result of a pipeline, you can use the Set-Content cmdlet to write the file as a single unit. For instance, you could read a file with Get-Content, use the .Replace() method to change something, and then write the file back using Set-Content:

![image](https://user-images.githubusercontent.com/47218880/61823737-fb03e100-ae21-11e9-9633-5c8fb84d438a.png)

Another useful feature of Set-Content is that you can set the encoding of the file using the –Encoding parameter. The possible values for the encoding are:

* ASCII (the default)
* BigEndianUnicode (UTF-16 big-endian)
* Byte
* String
* Unicode (UTF-16 little-endian)
* UTF7
* UTF8
* Unknown

With all these options, you should have no trouble writing in any format that you need.

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/05_Files_and_Folders/02_Creating_Files_Folders.md" > Continue to Next Topic </a>
