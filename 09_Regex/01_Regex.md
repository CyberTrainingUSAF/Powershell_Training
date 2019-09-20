# PowerShell’s regular expression 
PowerShell is built upon Microsoft’s .NET framework. In regular expressions, as in much else, PowerShell uses the .NET implementation. And .NET in turn essentially uses Perl 5’s regular expression syntax, with a few added features such as named captures. For someone familiar with regular expressions, especially as they are implemented in .NET or Perl, the difficulty in using PowerShell is not in the syntax of regular expressions themselves, but rather in using regular expressions to do work. Because PowerShell is new, detailed documentation and examples are harder to find than for .NET or Perl.

# Matching and replacing
PowerShell has -match and -replace operators that are roughly analogous to the m// and s/// operators in Perl. For example, start with the command
```
$greeting = "Hello world"
```
which would be legal PowerShell or Perl code. The PowerShell statement
```
$greeting -match "ello"
```
would return a Boolean true value just as the Perl statement
```
$greeting =~ m/ello/;
```
would return 1. Similarly, the PowerShell statement
```
$greeting -replace "world", "planet"
```
would return “Hello planet“, as would the statement
```
$greeting =~ s/world/planet/;
```
in Perl. However, the PowerShell statement returns a new string, leaving $greeting unchanged, while the corresponding  Perl statement changes the string $greeting in place.

# Case-sensitivity
Perl modifies the behavior of the m// and s/// operators by adding characters to indicate such things as case-sensitivity. Both operators, like nearly everything else in Perl, are case-sensitive by default. Appending an ‘i‘ causes them to be case-insensitive.

PowerShell’s -match and -replace operators, like nearly everything else in PowerShell, are case-insensitive by default. PowerShell offers the highly recommended option of specifying -imatch and -ireplace to make case-insensitivity explicit. The case-sensitive counterparts are -cmatch and -creplace.

# Retrieving single matches
After performing a match in Perl, the captured matches are stored in the variables $1, $2, etc. Similarly, after a match PowerShell creates an array $matches with $matches[n] corresponding to Perl’s $n.

# Global matching and replacing
Perl
Perl uses a ‘g‘ option to specify global matches and replacements. The unmodified operator m// returns a Boolean value, but the modified operator m//g returns an array containing all substrings matching the specified pattern. Both the s/// and s///g operators modify their string in place. The unmodified s/// operator replaces only the first match in the string, while the modified s///g operator replaces all occurrences of the pattern.

For example, first set
```
$str = "Cookbook";
```
Then executing
```
$b = $str =~ m/(\woo\w)/;
```
sets $b to 1 (true) and populates $1 with “Cook“. Executing
```
@a = $str =~ m/(\woo\w)/g;
```
sets @a to the array containing “Cook” and “book”.

The statement
```
$str =~ s/oo/u/;
```
would convert “Cookbook” into “Cukbook“, while the statement
```
$str =~ s/oo/u/g;
```
would convert “Cookbook” into “Cukbuk“.

# PowerShell
PowerShell’s -creplace and -ireplace work very much like the s///g and s///ig operators in Perl: all matches are replaced. PowerShell version 1.0 does not have a direct analog to Perl’s s/// and s///i operators without the ‘g‘ option.

Neither does PowerShell 1.0 have an analog to the m//g option in Perl, though Keith Hill has proposed that Microsoft add a -matches operator in a future release of PowerShell analogous to Perl’s m//g.

In order to have more fine control over match and replace operations in PowerShell, one must use the [regex] class rather than the -match and -replace operators. The following example shows how to do a global match in PowerShell.
```
$str = "Cookbook"
[regex]::matches($str, "\woo\w")
```
The last command returns a compound structure containing more information than just the matches. To fill an array $words with just the matches, use the command
```
$words = ([regex]::matches($str, "\woo\w") | %{$_.value})
```
which pipes the [regex]::matches output through the foreach operator % and selects the matched text.

The [regex] class in PowerShell is invoking the Regex class in .NET, and so all the options of the .NET class are available. For example, the .NET class library has a RegexOptions enumeration with useful values such as IgnoreCase, RightToLeft, etc. However, it is not obvious how one should translate from .NET to PowerShell syntax. You can’t write RegexOptions.IgnoreCase in PowerShell code the way you would in C#. It turns out the thing to do is to quote the option as a string. For example, the code
```
[regex]::matches($str, "[a-z]ook", "IgnoreCase")
```
will match “Cook” and “book“, whereas without the IgnoreCase option only “book” would match.

Replacing with captures
Often you want to replace a pattern not with a constant string but with a string based on the text that matched. For example, suppose you want to replace italicized words with double words. If $str contained “<i>big</i>” then after executing the Perl statement
```
$str =~ s{<i>(\w+)</i>}{$1 $1};
```
$str would contain “big big“. (This illustrates an alternative Perl syntax for s///. The s{}{} alternative prevents having to escape backslashes as in </i> above.)

The equivalent PowerShell code would be
```
$str = [regex]::Replace($str, "<i>(\w+)</i>", '$1 $1');
```
Notice the single quotes around the replacement pattern. This is to keep PowerShell from interpreting “$1” before passing passing it to the Regex class. We could use double quotes if we also put back ticks in front of the dollar signs to escape them.
