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
negVowelRegex = re.compile(r'^[aeiouAEIOU]') # negates the character class

```

**nb:** inside the square brackets, you don't have to escape any of the reserved chars!

## other special characters :

`^` means the string must start with pattern  
 `$` means the string must end with the pattern.   
Both means **the entire string** must match the entire pattern. `'^\d+$'`: begins and ends with that pattern   


The `.` dot is a _character wildcard_ ; it matches any character except newlines.
`*` means zero or more of a particular pattern. 
So, `.*` means anything, any pattern whatsoever. this is greedy by default
```python
nameRegex = re.compile(r'First Name: (.*) Last Name: (.*)') # group the any pattern...
nameRegex.finall('Fist Name: Al Last Name: Sweigart') # returns [('Al', 'Sweigart')] 
```
to make it non-greedy, make it `.*?`


Pass re.DOTALL as the second argument to re.compile() to make the . dot match newlines as well.
`dotStar = re.compile(r'.*', re.DOTALL)`

Pass re.I as the second argument to re.compile() to make the matching **case-insensitive**.
`vowelRegex = re.compile(r'[aeiou]', re.I)`

## sub() 

The `sub()` regex method will substitute matches with some other text.
Using `\1`,`\2` and so will substitute group 1, 2, etc in the regex pattern.
Passing `re.VERBOSE` lets you add whitespace and comments to the regex string passed to re.compile().
e.g. of adding documentation to a regex pattern:
```python
re.compile(r'''
\d\d\d   # area code
-        # first dash
\d\d\d   # first 3 digits
-        # second dash
\d\d\d\d # last 4 digits''', re.VERBOSE)
```

If you want to pass multiple arguments (re.DOTALL , re.IGNORECASE, re.VERBOSE), combine them with the `|` bitwise operator.
nb: the syntax for this bitwise operator shouldn't be generalised to other python stuff...


## example program: scraping phone numbers and emails from a pdf 


```python
#! /usr/bin/python3

import re, pyperclip 

# create regex for phone numbers
phoneRegex = re.compile(r'''
# examples of phone numbers: 415-555-0000, 555-0000, (415) 555-0000, 555-0000 ext 12345, ext. 12345, x12345
( # prevents the output from being a list of tuples 
((\d\d\d) |  (\(\d\d\d\)))?  # area code(optional)
(\s|-)                       # first separator
(\d\d\d)                     # first 3 digits 
-                            # separator 
\d\d\d\d                     # last 4 digits 
    (((ext(\.)?\s) | x)   # optional extension, the word part
     (\d{2,5}))?          # optional extention, the number part the digits after can be b/w 2 to 5 digits
)
''', re.VERBOSE)

# create regex for email addr:
emailRegex = re.compile(r'''
# examples: something.+_@something.com
[a-zA-Z0-9_.+]+  # name part, create your own class
@                # @ symbol 
[a-zA-Z0-9_.+]+  # domain name part
''', re.VERBOSE)


#get text off the clipboard
inputText = pyperclip.paste()


# extract email/phone from the text
extractedPhone = phoneRegex.findall(inputText)
extractedEmail = emailRegex.findall(inputText)
        # do some printing to doublecheck on a small sample size: 
        #  print(extractedPhone) # shows the output will be as a list of tuples, so to avoid, encase everything in a larger group
        #  print(extractedEmail)
        #todo: copy extracted phone numbers and emails to hte clipboard

allPhoneNumebers = [] # init a blank list
for phoneNumber in extractedPhone : 
    allPhoneNumebers.append(phoneNumber[0]) # append the first of the tuple..

# print allPhoneNumebers test output
results = '\n'.join(allPhoneNumebers)  # puts everything as one phone number per line
        + '\n'
        + '\n'.join(extractedEmail)
pyperclip.copy(results);

```


## File IO  

- os module with path function and it's useful commands: `cwd()` ...
```python
import os 
os.path.join("home", "folder", "sub1", "subsub1"); # outputs path w the correct separator
```

- absolute and relative filepaths

Files have a name and a path.  
The root folder is the lowest folder.  
In a file path, the folders and filename are separated by backslashes on Windows and forward slashes on Linux and Mac.   
Use the `os.path.join()` function to combine folders with the correct slash.  
The current working directory is the oflder that any relative paths are relative to.  
`os.getcwd()` will return the current working directory.  
`os.chdir()` will change the current working directory.  
Absolute paths begin with the root folder, relative paths do not.  
The . folder represents "this folder", the .. folder represents "the parent folder".  
`os.path.abspath()` returns an absolute path form of the path passed to it.  
`os.path.relpath(<abs path 1>,<abs path 2>)` returns the relative path between two paths passed to it.  
`os.makedirs()` can make folders.  
`os.path.getsize()` returns a file's size.  
`os.listdir()` returns a list of strings of filenames.  
`os.path.exists()` returns True if the filename passed to it exists.  
`os.path.isfile()` and `os.path.isdir()` return True if they were passed a filename or file path.  



### r/w textfiles

The `open()` function will return a file object which has reading and writing –related methods.    
    Pass ‘`r`' (or nothing) to open() to open the file in read mode.   
    Pass ‘`w`' for write mode. Pass ‘`a`' for append mode. write will overwrite!  
Opening a nonexistent filename in write or append mode will create that file.  
Call `read()` or `write()` to read the contents of a file or write a string to a file.  
Call `readlines()` to return a _list of strings_ of the file's content.  
Call `close()` when you are done with the file.  
The `shelve` module can store Python values in a binary file. Has a **dictionary like structure**.
The `shelve.open()` function returns a dictionary-like shelf value.  


### file organisation: moving, copying
- use `shutil` module 
- `shutil.copy()` and `shutil.copytree()` tree will copy entire directory structure
- `shutil.move()`


### file deletion via the `os` module


permanent deletes: 
- `os.unlink(<string of filename>)`
- `os.rmdir(<folder name>)`  **nb:** the folder has to be empty
- use `shutil`'s `rmtree()` to recursively delete...

use `send2trash` 3rd party module : `send2trash.send2trash(<filename>)`

### walking thru a dir

- by using `os.walk(<root folder>)`, using it's return value in a for loop, general way to walk thru: 

```python
import os 
for folderName, subfolders, filenames in os.walk("<root folder>"):
    print("the folder is " + folderName)
    print("the subfolders in " + folderName + " are: " + str(subfolders))
    print("the filenames in " + folderName + " are: " + str(filenames))
    print()
```

add control flow to them to control what you wanna do. The os.walk function will therefore be v useful. 
rmb to do dry runs!