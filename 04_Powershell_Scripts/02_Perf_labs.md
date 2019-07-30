<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## Working with Strings

1. Open Windows PowerShell.

2. Create a variable called $a and assign the value this is the beginning to it. The code for this is shown here.
```
$a = "this is the beginning"
```
3. Create a variable called $b and assign the number 22 to it. The code for this is shown here.
```
$b = 22
```
4. Create a variable called $c and make it equal to $a + $b. The code for this is shown here.
```
$c = $a + $b
```
5. Print out the value of $c. The code for this is shown here.
```
$c
```
6. The results of printing out c$ are shown here.
```
this is the beginning22
```
7. Modify the value of $a. Assign the string this is a string to the variable $a. This is shown here.
```
$a = "this is a string"
```
8. Press the Up Arrow key and retrieve the $c = $a + $b command.
```
$c = $a + $b
```
9. Now print out the value of $c. The command to do this is shown here.
```
$c
```
10. Assign the string this is a number to the variable $b. The code to do this is shown here.
```
$b = "this is a number"
```
11. Press the Up Arrow key to retrieve the $c = $a + $b command. This will cause Windows PowerShell to reevaluate the value of $c. This command is shown here.
```
$c = $a + $b
```
12. Print out the value of $c. This command is shown here.
```
$c
```
13. Change the $b variable so that it can contain only an integer. (Commonly used data type aliases are shown in Table 5-4.) Use the $b variable to hold the number 5. This command is shown here.
```
[int]$b = 5
```
![image](https://user-images.githubusercontent.com/47218880/61741036-a3983f00-ad55-11e9-893f-986bcd3c37ad.png)

14. Assign the string this is a string to the $b variable. This command is shown here.
```
$b = "this is a string"
```
Attempting to assign a string to a variable that has an [int] constraint placed on it results in the error shown here (these results are wrapped for readability).
```
Cannot convert value "this is a number" to type "System.Int32".
Error: "Input string was not in a correct format."
At line:1 char:3
+ $b  <<<< = "this is a string"

```

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/04_Powershell_Scripts/03_Loops.md" > Continue to Next Topic </a>












