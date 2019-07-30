<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Points to Ponder

Whenever it seems appropriate, we wrap up each chapter with a brief section that covers some of the common mistakes we see when we teach classes. The idea is to help you see what most often confuses other administrators like yourself, and to avoid those problems—or at least to be able to find a solution for them—as you start working with the shell.

# Cmdlet names

First up is the typing of cmdlet names. It’s always verb-noun, like Get-Content. All of these are options we’ll see newcomers try, but they won’t work:  
```
Get Content    
GetContent    
Get=Content    
Get_Content
```

Part of the problem comes from typos (= instead of -, for example), and part from verbal laziness. We all pronounce the command as Get Content, verbally omitting the dash. But you have to type the dash.

**Parameters**

Parameters are also consistently written. A parameter that takes no value, such as -recurse, gets a dash before its name. You need to have spaces separating the cmdlet name from its parameters, and the parameters from each other. The following are correct:
```
Dir -rec (the shortened parameter name is fine)
New-PSDrive -name DEMO -psprovider FileSystem -root \\Server\Share
```
But these examples are all incorrect:
```
Dir-rec (no space between alias and parameter)
New-PSDrive -nameDEMO (no space between parameter name and value)
New-PSDrive -name DEMO-psprovider FileSystem 
        (no space between the first parameter’s value and the second parameter’s name)
```
PowerShell isn’t normally picky about upper- and lowercase, meaning that dir and DIR are the same, as are -RECURSE and -recurse and -Recurse. But the shell sure is picky about those spaces and dashes.

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Continue to Next Topic </a>
