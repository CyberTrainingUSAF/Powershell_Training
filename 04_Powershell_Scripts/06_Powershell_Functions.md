<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/00-Table-of-Contents.md" > Return to TOC </a>

---

## FUNDAMENTALS OF POWERSHELL FUNCTIONS

In this section we’ll cover the basic concepts and features of PowerShell functions. Functions are the most lightweight form of PowerShell command. They exist in memory only for the duration of a session. When you exit the shell session, the functions are gone. They’re also simple enough that you can create useful functions in a single line of code.

**Functions and scriptblocks**

A function, at its simplest, is defined as follows:

```
function <name> {<statement list>}
```  

A scriptblock, at its simplest, is defined like this:

```
{<statement list>}
```

In both cases the braces contain a list of PowerShell statements that are executed when the function or scriptblock is invoked. You’ve seen scriptblocks used with Where-Object and Foreach-Object or the looping and conditional statements in previous chapters. Scriptblocks are covered in more detail in chapter 10.

Looking at the two, you can describe a function as a named scriptblock or a scriptblock as an anonymous function—we prefer the latter.

We’ll start by working through a number of examples showing you how to create simple functions. Take a look at our first example:

```
PS> function hello { 'Hello world' }
```

In this example, hello is a function because it’s preceded by the function keyword. This function should emit the string “Hello world”. Execute it to verify this:

```
PS> hello
Hello world
```

Yes, it works exactly as expected. You’ve created your first command.

Okay, that was easy. Now you know how to write a simple PowerShell function. The syntax is shown in the figure below.

 The simplest form of a function definition in PowerShell
 
![image](https://user-images.githubusercontent.com/47218880/61804144-068ee200-adf9-11e9-88ef-ee85ec2aee24.png)

A function that writes only “Hello world” isn’t too useful. Let’s see how to personalize this function by allowing an argument to be passed in.

**Passing arguments using $args**

The ability to pass values into a function is called parameterizing the function. In most languages, this means modifying the function to declare the parameters to process. For simple PowerShell functions, you don’t have to do this because there’s a default argument array that contains all the values passed to the function. This default array is available in the variable $args. Here’s the previous hello example modified to use $args to receive arguments:
```
PS> function hello { "Hello there $args, how are you?" }
PS> hello Bob
Hello there Bob, how are you?
```

String expansion inserts the value stored in $args into the string that’s emitted from the hello function. Now let’s see what happens with multiple arguments:

```
PS> hello Bob Alice Ted Carol
Hello there Bob Alice Ted Carol, how are you?
```

Following the string expansion rules described in chapter 2, the values stored in $args get interpolated into the output string with each value separated by a space or, more specifically, separated by whatever is stored in the $OFS variable.

Note
Both $args and $OFS are described in the help file about_Automatic_Variables.

So, let’s take one last variation on this example. We’ll set $OFS in the function body with the aim of producing a more palatable output:

```
PS> function hello
{
$ofs=","
"Hello there $args and how are you?"
}

PS> hello Bob Carol Ted Alice
Hello there Bob,Carol,Ted,Alice and how are you?
```

That’s better. Now at least you have commas between the names. Let’s try it again, with commas between the arguments:

```
PS> hello Bob,Carol,Ted,Alice
Hello there System.Object[] and how are you?
```

This isn’t the result you were looking for. What happened? Let’s define a new function:

```
PS> function count-args {
    "`$args.count=" + $args.count
    "`$args[0].count=" + $args[0].count
}
```

This function will display the number of arguments passed to it as well as the number of elements in the first argument. First, you use it with three scalar arguments:

```
PS> count-args 1 2 3
$args.count=3
$args[0].count=1
```
As expected, it shows that you passed three arguments. It shows a value of 1 for the Count property on $args[0] because $args[0] is a scalar (the number 1) which has a Count property of 1 by default. Try it with a comma between each of the arguments:

```
PS> Count-Args 1,2,3
$args.count=1
$args[0].count=3
```

Now you see that the function received one argument, which is an array of three elements. Finally, try it with two sets of comma-separated numbers:

```
PS> count-args 1,2,3 4,5,6,7
$args.count=2
$args[0].count=3

```
The results show that the function received two arguments, both of which are arrays. The first argument is an array of three elements and the second is an array with four elements. The comma here works like the binary comma operator in expressions.

Two values on the command line with a comma between them will be passed to the command as a single argument. The value of that argument is an array of those elements. This applies to any command, not only functions. Now let’s look at a couple of examples where $args enables simple but powerful scenarios.

---

<a href="https://github.com/CyberTrainingUSAF/Powershell_Training/blob/master/04_Powershell_Scripts/07_Functions_Perf_lab.md" > Continue to Performance Labs </a>
