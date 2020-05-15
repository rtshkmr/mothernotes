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


## Python Debugging: 

- Raising exceptions via the `raise Exception(<string message>)` keyword.
- using traceback to trace the stack frames... just check documentation for this. Direct to a textfile to keep error logs!

You can raise your own exceptions: raise Exception(‘This is the error message.')
You can also use assertions: assert condition, ‘Error message'
Assertions are for detecting programmer errors that are not meant to be recovered from. User errors should raise exceptions.

### logging in python

basically an easy way to create markers in your code to help you trace the flow of logic...
There are 5 log levels, read up when needed.

The logging module lets you display logging messages.
Log messages create a "breadcrumb trail" of what your program is doing.
After calling `logging.basicConfig()` to set up logging, call `logging.debug(‘This is the message')` to create a log message.
When done, you can disable the log messages with `logging.disable(logging.CRITICAL)` at the top of the file
Don't use `print()` for log messages: It's hard to remove them all when you're done debugging.
The five log levels are: DEBUG, INFO, WARNING, ERROR, and CRITICAL.
You can also log to a file instead of the screen with the filename keyword argument in the logging.basicConfig() function.



## [Python Webscraping](https://automatetheboringstuff.com/chapter11/)

haha this is so hardcody hahaha

### `webbrowser` module

demo-ing the webbrowser module: input is a call to the script that takes in a location of some sort and then opens up googles maps to that address...

```python
import webbrowser, sys , pyperclip

sys.argv # for the cmd args, passed in as a list of strings...

#============    obtain addresss =========================================
# now we wanna make it into a single query and join the elements in that list: 
if len(sys.argv) > 1 : # e.g. argv: ['mapit', '870', 'valencia', 'street']
    address = ' '.join(sys.argv[1:])
else: # take from clipboard using pyperclip
    address = pyperclip.paste()


# observe url inputs and just figure out how to make that query..
webbrowser.open('https://www.google.com/maps/place' + address);
```
make this executable and add to paths and now can just run this easily.


### python requests module (3rd party)

won't be good for doing auth and other gui-related actions...

[documentation here](http://requests.readthedocs.org/)

The Requests module is a third-party module for downloading web pages and files.
`requests.get()` returns a **Response object**.
The `raise_for_status()` Response method will raise an exception if the download failed.
You can save a downloaded file to your hard drive with calls to the `iter_content()` method.

to save the response object into a file, note that the response object is binary data and so, you pass in args like so: 
`playFile = open('myFile.txt', 'wb')` where the `wb` stands for "write binary"

```python
res = requests.get('https://automatetheboringstuff.com/files/rj.txt')
playFile = open('RomeoAndJuliet.txt', 'wb')
for chunk in res.iter_content(100000) #number reflects bytes
    playFile.write(chunk)
playFile.close() #rmb to close the file
```

### HTML Scraping via the `BeautifulSoup` module `bs4`

Web pages are plaintext files formatted as HTML.
HTML can be parsed with the `BeautifulSoup` module.
BeautifulSoup is imported with the name `bs4`.
Pass the string with the HTML to the `bs4.BeautfiulSoup()` function to get a Soup object. good to pass in a 2nd arg for the markup parser e.g. `'html.parser'`
The Soup object has a `select()` method that can be passed a string of the CSS selector for an HTML tag.
You can get a CSS selector string from the browser's developer tools by right-clicking the element and selecting Copy CSS Path.
The `select()` method will return a list of matching Element objects.

```python
import bs4, requests

URL = 'https://www.amazon.com/s?bbn=493964&rh=n%3A172282%2Cn%3A%21493964%2Cn%3A502394%2Cp_n_shipping_option-bin%3A3242350011&dc&fst=as%3Aoff&pf_rd_i=16225009011&pf_rd_m=ATVPDKIKX0DER&pf_rd_p=82d03e2f-30e3-48bf-a811-d3d2a6628949&pf_rd_r=9XH4WCPW90VPCBR9GBM4&pf_rd_s=merchandised-search-4&pf_rd_t=101&qid=1486423355&qid=1486423355&rnid=493964&rnid=493964%2Fs%2Fref%3Dsr_nr_n_1%3Ffst%3Das%3Aoff&ref=s9_acss_bw_cts_AEElectr_T2_w'
chosenCSSPath = 'html.a-js.a-audio.a-video.a-canvas.a-svg.a-drag-drop.a-geolocation.a-history.a-webworker.a-autofocus.a-input-placeholder.a-textarea-placeholder.a-local-storage.a-gradients.a-transform3d.a-touch-scrolling.a-text-shadow.a-text-stroke.a-box-shadow.a-border-radius.a-border-image.a-opacity.a-transform.a-transition.a-ember body.a-m-us.a-aui_72554-c.a-aui_dropdown_187959-c.a-aui_pci_risk_banner_210084-c.a-aui_perf_130093-c.a-aui_preload_261698-c.a-aui_tnr_v2_180836-c.a-aui_ux_145937-c.a-meter-animate div#a-page div#search.s-desktop-container div.s-desktop-width-max.s-desktop-content.sg-row div.sg-col-20-of-24.sg-col-28-of-32.sg-col-16-of-20.sg-col.sg-col-32-of-36.sg-col-8-of-12.sg-col-12-of-16.sg-col-24-of-28 div.sg-col-inner span.rush-component.s-latency-cf-section div.s-result-list.s-search-results.sg-row div.sg-col-20-of-24.s-result-item.s-asin.sg-col-0-of-12.sg-col-28-of-32.sg-col-16-of-20.sg-col.sg-col-32-of-36.sg-col-12-of-16.sg-col-24-of-28 div.sg-col-inner span.celwidget.slot=SEARCH_RESULTS.template=SEARCH_RESULTS.widgetId=search-results div.s-include-content-margin.s-border-bottom.s-latency-cf-section div.a-section.a-spacing-medium div.sg-row div.sg-col-4-of-12.sg-col-8-of-16.sg-col-16-of-24.sg-col-12-of-20.sg-col-24-of-32.sg-col.sg-col-28-of-36.sg-col-20-of-28 div.sg-col-inner div.sg-row div.sg-col-4-of-24.sg-col-4-of-12.sg-col-4-of-36.sg-col-4-of-28.sg-col-4-of-16.sg-col.sg-col-4-of-20.sg-col-4-of-32 div.sg-col-inner div.a-section.a-spacing-none.a-spacing-top-small div.a-row.a-size-base.a-color-base div.a-row a.a-size-base.a-link-normal.a-text-normal span.a-price span'
def getAmazonPrice(productURL):
    res = request.get(productURL) #dl that page
    res.raise_for_status() # check for success in downloading

    soup = bs4.BeautifulSoup(res.text, 'html.parser')
    elems = soup.select(chosenCSSPath)
    elems[0].text.strip()

price = getAmazonPrice(URL);
print("the price is : " + price)
```

### [selenium module's](https://selenium-python.readthedocs.org) webdriver to control browser behaviour (3rd party)

To import selenium, you need to run: "`from selenium import webdriver`" (and not "import selenium").
To open the browser, run: `browser = webdriver.Firefox()`
To send the browser to a URL, run: `browser.get(‘https://inventwithpython.com')`
The `browser.find_elements_by_css_selector()` method will return a list of WebElement objects: `elems = browser.find_elements_by_css_selector(‘img')`
**nb:** all the functions have a singular and a plural form.

WebElement objects have a "text" variable that contains the element's HTML in a string: elems[0].text
The `click()` method will click on an element in the browser.
The `send_keys(<string inputs>)` method will type into a specific element in the browser.
The `submit()` method will simulate clicking on the Submit button for a form.
The browser can also be controlled with these methods: `back()`, `forward()`, `refresh()`, `quit()`.

## Common Document Handling

### Excel via `openpyxl`

**reading**
The OpenPyXL third-party module handles Excel spreadsheets (`.xlsx` files).
`openpyxl.load_workbook(filename)` returns a Workbook object.
`get_sheet_names()` and `get_sheet_by_name()` help get Worksheet objects.
The square brackets in sheet`[‘A1']` get Cell objects.
Cell objects have a "value" member variable with the content of that cell.
The `cell()` (two input args: e.g. row=1, column=2) method also returns a Cell object from a sheet.


**modifying:**
You can view and modify a sheet's name with its "title" member variable.
Changing a cell's value is done using the square brackets, just like changing a value in a list or dictionary.
Changes you make to the workbook object can be saved with the save() method.

You can view and modify a sheet's name with its "title" member variable.
Changing a cell's value is done using the square brackets (and assigning the value you want), just like changing a value in a list or dictionary.
Changes you make to the workbook object can be saved with the `save()` method. **NB:** to prevent buggy overwriting, save as a new filename!


### PDFs via `PyPDF2`

note that pdfs are binary files and it makes it complicated to handle via code
The PyPDF2 module can read and write PDFs **for editing done at a page level!**
Opening a PDF is done by calling `open()` and passing the file object to `PdfFileReader()` 
```python 
    pdfFile = open('myFile.pdf', 'rb');
    reader = PyPDF2.PdfFileReader(pdfFile)
```
A Page object can be obtained from the PDF object with the `getPage()` method.
The text from a Page object is obtained with the `extractText()` method, which can be imperfect.
New PDFs can be made from `PdfFileWriter()`.
New pages can be appended to a writer object with the `addPage()` method.
Call the `write()` method to save its changes.


### .docx files 

The `Python-Docx` third-party module can read and write .docx Word files.
Open a Word file with `docx.Document()`
Access one of the Paragraph objects from the "paragraphs" member variable, which is a list of Paragraph objects.
Paragraph objects have a "text" member variable containing the text as a string value.
Paragraphs are composed of "runs".  The "runs" member variable of a Paragraph object contains a list of Run objects.
Run objects also have a "text" member variable.
Run objects have a "bold", "italic", and "underline" member variables which can be set to True or False.
Paragraph and run objects have a "style" member variable that can be set to one of Word's built-in styles.
Word files can be created by calling add_paragraph() and add_run() to append text content.

