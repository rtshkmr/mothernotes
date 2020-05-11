to document the gaining of some basic python skills, resources: [Automate the Boring Stuff](http://automatetheboringstuff.com/) and the Pentester academy's instructions.

# [Scripting w Python](http://automatetheboringstuff.com/appendixb/)

Shebangs followed by filepath to the python interpreter, OS dependent syntax: 
    * win: `#! python3`
    * linux: `#! /usr/bin/python3`
  
### windows specifics

There's a `py.exe` program that selects the correct python interpreter to use. It repls the script.
Create a bat file. Namespace it properly and put in some folder that is added to the env variables, 
for easy calling for all `.bat` and `.py`  scripts.

Creating batch file: add to head of textfile `@py.exe C:\path\to\your\pythonScript.py %*` and save as `.bat`

 note: using the `@pyw` program can make the script run window-less

# [regex in python](http://automatetheboringstuff.com/2e/chapter7/)

* the `re` module for regex. 
* use raw strings to pass in params to the functions in the re module.


## Regex Grouping and Piping

````python
# demos the use of parentheses for grouping regexes:
#___________________________________________________
import re 
phoneNumRegex = re.compile(r"\d\d\d-\d\d\d-\d\d\d") #without grouping
matchedObject1 = phoneNumRegex.search("input string: number is 999-000-111") # w/o grouping
matchedObject2 = phoneNumRegex.search("input string: number is (999)-(000)-(111)") # w grouping
matchedObject1.group() # returns: 999-000-111 as a single ungrouped string
matchedObject2.group() # returns string that's grouped and 1-indexed

# demos the use of pipes for alternative suffixes of a string:
#_____________________________________________________________
batRegex = re.compile(r"Bat (man|mobile|copter|bat)") #note the grouping of piped strings
mo = batRegex.search("Batmobile lost a wheel") # returns 'Batmobile'

````

**nb:** can nest the groups!


## Regex and Repetitions

### ? : match zero or one times (appear once or not at all)
The ? says the group matches zero or one times.
The * says the group matches zero or more times.
The + says the group matches one or more times.
The curly braces can match a specific number of times. 

```python
haRegex = re.compile(r'(Ha){3}') # curly brace for a specific number of times
haRegex.search("he said hahaha")

variableHasRegex = re.compile(r'(Ha){3,5}') # curly brace for a specific number of times within range, is greedy by default
lowerbounded = re.compile(r'(Ha){3,}') # lowerbounded
nongreedy = re.compile(r'(Ha){3,5}?)

```

The curly braces with two numbers matches a minimum and maximum number of times.
Leaving out the first or second number in the curly braces says there is no minimum or maximum.
"Greedy matching" matches the longest string possible, "nongreedy matching" (or "lazy matching") matches the shortest string possible.
Putting a question mark after the curly braces makes it do a nongreedy/lazy match.


## Regex findall()

findall() returns a list of string objects that match the regex while search() returns Match objects

**nb:** if there are zero or one group inside the regex then we get list of strings. else with more than 1 group, findall() will return a n-tuple


## Regex character classes 

to describe the type of character. There's a general style to it: lowercase vs uppercase(it becomes a negation)

### making own character class

```python
vowelRegex = re.compile(r'[aeiouAEIOU]')
negVowelRegex = re.compile(r'^[aeiouAEIOU]') # negates the 
````