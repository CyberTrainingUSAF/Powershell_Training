# Literal characters
The best place to begin is with the simplest of expressions: expressions that contain no special characters. These expressions contain what are known as literal characters. A literal character is anything except ```[\^$.|?*+(). Special characters must be escaped using \ to avoid errors, for example:```
```
'9*8'-match '\*'   # * is reserved 
'1+5' -match '\+'   # + is reserved 
```
Curly braces ({}) are considered literal in many contexts.
```
Except when...:
Curly braces become reserved characters if they enclose either a number, two numbers separated by a comma, or one number followed by a comma.
In the following two examples, { and } are literal characters:
'{string}' -match '{'
'{string}' -match '{string}'
In the preceding example, the curly braces take on a special meaning. To match, the string would have to be string followed by 123 of the character "g". We will explore 'string{123}' -match 'string{123}'
{} in detail when discussing repetition.
```
The following statement returns true and fills the matches automatic variable with what matched. The matches variable is a hash table; it is only updated when something successfully matches when using the match operator:
```
PS> 'The first rule of regex club' -match 'regex'

True

PS> $matches

Name                           Value 
----                           ----- 
0                              regex
```
If a match fails, the matches variable will continue to hold the last matching value:
```
PS> 'This match will fail' -match 'regex'

False

PS> $matches

Name                                             Value 
----                                             ----- 
0                                                regex 
```
