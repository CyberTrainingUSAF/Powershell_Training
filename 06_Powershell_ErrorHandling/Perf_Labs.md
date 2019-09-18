## USING PROMPTFORCHOICE TO LIMIT SELECTIONS AND USING TRY...CATCH...FINALLY: STEP-BY-STEP EXERCISES
### This exercise explores the use of PromptForChoice to limit selections in a script. Following this exercise, you will explore using Try...Catch...Finally to detect and to catch errors.

#### Exploring the PromptForChoice construction

1. Open the Windows PowerShell ISE.

2. Create a new script called PromptForChoiceExercise.ps1.

3. Create a variable to be used for the caption. Name the variable caption and assign a string value of This is the caption to the variable. The code to do this is shown here.


```
$caption = "This is the caption"
```
4. Create another variable to be used for the message. Name the variable message and assign a string value of This is the message to the variable. The code to do this is shown here.


```
$message = "This is the message"
```
5. Create a variable named choices that will hold the ChoiceDescription object. Create an array of three choices—choice1, choice2, and choice3—for the ChoiceDescription object. Make the default letter for choice1 c, the default letter for choice2 h, and the default letter for choice3 o. The code to do this is shown here.

```

$choices = [System.Management.Automation.Host.ChoiceDescription[]] `
 @("&choice1", "c&hoice2", "ch&oice3")
```
6. Create an integer variable named defaultChoice and assign the value 2 to it. The code to do this is shown here.
```
[int]$DefaultChoice = 2
```
7. Call the PromptForChoice method and assign the return value to the ChoiceRTN variable. Provide PromptForChoice with the caption, message, choices, and defaultChoice variables as arguments to the method. The code to do this is shown here.


```
$choiceRTN = $host.ui.PromptForChoice($caption,$message, $choices,$defaultChoice)
```
8. Create a switch statement to evaluate the return value contained in the choiceRTN variable. The cases are 0, 1, and 2. For case 0, display a string that states choice1. For case 1, display a string that states choice2, and for case 2, display a string that states choice3.

The switch statement to do this is shown here.
```
switch($choiceRTN)
 {
  0    { "choice1"  }
  1    { "choice2"  }
  2    { "choice3"  }
 }
```
9. Save and run the script. Test each of the three options to ensure that they work properly. This will require you to run the script three times and select each option in sequence.

This concludes the exercise.
