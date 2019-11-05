# Python 

## Development intro 
> Note: This course uses Python 3.7 and you will definitely run into issues if you are using Python < 3.7.
Download and Install Python 3 from Source
https://www.python.org/downloads/

Here are the commands that we'll run to build and install Python 3 Ubuntu 18.04 :
```
$ sudo -i
$ apt update -y
$ apt install -y \
  wget \
  build-essential \
  libffi-dev \
  libgdbm-dev \
  libc6-dev \
  libssl-dev \
  zlib1g-dev \
  libbz2-dev \
  libreadline-dev \
  libsqlite3-dev \
  libncurses5-dev \
  libncursesw5-dev \
  xz-utils \
  tk-dev
$ cd /usr/src
$ wget http://python.org/ftp/python/3.7.2/Python-3.7.2.tar.xz
$ tar xf Python-3.7.2.tar.xz
$ cd Python-3.7.2
$ ./configure --enable-optimizations
$ make altinstall
```
> Note: make altinstall causes it to not replace the built in python executable.

Ensure that secure_path in /etc/sudoers file includes /usr/local/bin. The line should look something like this:
```
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin
```
Upgrade Pip (might not be necessary)

The version of pip that we have might be up-to-date, but it's a good practice to try to update it after the installation. We need to use the pip3.7 executable because we're working with Python 3, and we use sudo so that we can write files under the /usr/local directory.
```
$ pip3.7 install --upgrade pip
```

## Setting up VIM for python 3
To get started with the course you're going to need a few things installed:

    Python 3
    Git
    A text editor of your choice

My preferred text editor is Vim and that's what I'll be using throughout the course.

This course is focused on using Python on a Linux system. You can definitely follow along on a Mac or Windows machine, but we won't be covering potential differences with those systems or how to install Python on Windows.

I will be using CentOS 7 and if you'd like to follow along exactly we'll go through what you need to be able to use one of your Linux Academy Cloud Playground servers.
Installing Development tools and Vim

We've already installed most of what we need, but attempting to install development tools again won't hurt. Vim has yet to be installed, so we'll add that now:

$ sudo yum update -y
$ sudo yum groupinstall -y "development tools"
$ sudo yum install -y vim-enhanced

Configure git:

This is just a best practice, but we'll want to configure Git to have our name and email address so that we can sign any commits that we make as we're developing.

$ git config --global user.name "Keith Thompson"
$ git config --global user.email "keith@linuxacademy.com"

Pull down sample bashrc

Having a comfortable development environment makes life easier. For this course, I'm going to use a slightly customized bash configuration to improve my experience in the terminal. You can chose to skip this step if you'd like. We can download this file from this course's Github repository:

$ curl https://raw.githubusercontent.com/linuxacademy/content-intro-to-python-development/master/helpers/bashrc -o ~/.bashrc
$ exec $SHELL

The exec $SHELL reloaded our shell session and we can now see the customizations to the prompt. We'll come back to this in a second to see some enhancements that only show up when working within git repositories.
Pull down sample vimrc

Vim is a great text editor, but it doesn't have the best default settings. I've created a simple, yet usable vim configuration to use with this course that will improve our experience while writing Python code. Let's fetch this file now:

$ curl https://raw.githubusercontent.com/linuxacademy/content-intro-to-python-development/master/helpers/vimrc -o ~/.vimrc

Demonstrat PS1 changes

One of the big changes in the bash configuration is that it adds support for showing information about the git repository that we're currently working in. Let's create a sample project so we can see what it shows:

$ mkdir sample
$ cd sample
$ touch file.txt
$ git init
$ git add --all .
$ git commit -m 'Initial commit'

## REPL
Python is an interpreted language, and code is evaluated line-by-line. Since each line can be evaluated by itself, the time between evaluating each line doesn't matter, and this allows us to have a REPL.
Documentation

    Python Interpreter

What is a REPL?

REPL stands for: Read, Evaluate, Print, Loop

Each line is read and evaluated. The return value is then printed to the screen, and then the process repeats.

Python ships with a REPL, accessible by running python3.7 from a terminal.

$ python3.7
Python 3.7.2 (default, Jan 15 2019, 19:31:18)
[GCC 4.8.5 20150623 (Red Hat 4.8.5-36)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>

The >>> indicates a line to type in, like a command prompt for Python. Later on we'll see a ..., which means that we are currently sitting in a scoped area. The only way out is to enter a blank line (no spaces) before the interpreter will evaluate the entire code block.

The simplest use of this would be to do some math:

>>> 1 + 1
2
>>>

2 is the return value of the expression and it is then printed to the screen. If something doesn't have a return value, then nothing will be printed to the screen and we'll just land back at a >>> prompt. We'll cover this later, but an example would be None:

>>> None
>>>
## Creating and Running Python Files
To exit the REPL, we can either type exit() (the parentheses are important) or hit Ctrl+d on your keyboard.

Since this is a course about Python scripting, we will be writing the majority of our code in scripts instead of using the REPL. To create a Python script, we can create a file ending with the file extension of .py.
Creating Our First Python Script

Let's create our first script to write our obligatory "Hello, World!" program:

$ vim hello.py

From inside this file we can enter the lines of Python that we need. For the "Hello, World!" example we only need

print("Hello, World!")

There are a few different pays that we can run this file. The first is by passing it to the python3.7 CLI.

$ python3.7 hello.py
Hello, World!

Setting a Shebang

You'll most likely want your scripts to be:

    Executable from anywhere (in our $PATH)
    Executable without explicitly using the python3.7 CLI

Thankfully, we can set the process to interpret our scripts by setting a shebang at the top of the file:

hello.py

#!/usr/bin/env python3.7
print("Hello, World")

We're not quite done, because now we need to make the file executable using chmod:

$ chmod u+x hello.py

Run the script now by using ./hello.py and we'll see the same result. If we'd rather not have a file extension on our script we can now remove that since we've put a shebang in the file. Renaming it to get rid of the extension (mv hello.py hello) and running ./hello will still result in Python 3.7 executing the script.
Adding Scripts to Our $PATH

Now we need to make sure that we can put this in our $PATH. For this course, we'll be using a bin directory in our $HOME folder to store our custom scripts, but scripts can go into any directory that is in your $PATH.

Let's create a bin directory and move our script:

$ mkdir ~/bin
$ mv hello.py ~/bin/hello

Here's how we add this directory to the $PATH in our .bashrc (the .bashrc for this course already contains this):

$ export PATH=$PATH:$HOME/bin

Finally, let's run the hello script from our $PATH:

$ hello
Hello, World!

## Using Comments
When writing scripts, we often want to leave ourselves notes or explanations. Python (along with most scripting languages) uses the # charater to signify that the line should be ignored and not executed.
Single Line Comment

We can comment out a whole line:
```
# This is a full line comment
```
or we can comment at the end of a line:
```
2 + 2 # This will add the numbers
```
What About Block Comments?

Python does not have the concept of block commenting that you may have encountered in other langauges. Many people mistake a triple quoted string as being a comment, but it is not, it's a multi-line string. That being said, multi-line strings can functionally work like comments, but they will still be allocated into memory.
```
"""
This is not a block comment,
but it will still work when you really need
for some lines of code to not execute.
"""
```
 This will allocate string in the memory. 

## Strings
Let's learn about one of the core data types in Python: the str type.

> Note:
\ - backslash
/ - forward slash

Documentation
    [strings (the str type)] https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str

Strings

Open a REPL to start exploring Python strings:
```
$ python3.7
```
We've already worked with a string when we created our "Hello, World!" program. We create strings using either single quotes ('), double quotes ("), or triple single or double quotes for a multi-line string.
```
>>> 'single quoted string'
'single quoted string'
>>> "double quoted string"
'double quoted string'
>>> '''
... this is a triple
... quoted string
... '''
'\nthis is a triple\nquoted string\n'
```
Strings also work with some arithmetic operators.

We can combine strings using the + operator and multiply a string by a number using the * operator:
```
>>> "pass" + "word"
'password'
>>> "Ha" * 4
'HaHaHaHa'
```
A string is a sequence of characters grouped together. We do need to cover the concept of an "Object" in object oriented programming before moving on. An "object" encapsulates two things 1) State & 2) behavior. For the built in types, the state makes sense because it's the entire contents of the object. The behavior aspect means that there are functions that we can call on the instances of the objects that we have. A function bound to an object is called a "method". Here are some example methods that we can call on strings:

find locates the first instance of a character (or string) in a string. This function returns the index of the character or string.
```
>>> "double".find('s')
-1
>>> "double".find('u')
2
>>> "double".find('bl')
3
```
lower converts all of the characters in a string to their lowercase versions (if they have one). This function returns a new string without changing the original, and this becomes important later.
```
>>> "TeStInG".lower() # "testing"
'testing'
>>> "another".lower()
'another'
>>> "PassWord123".lower()
'password123'
```
Lastly, if we need to use quotes or special characters in a string we can do that using the '' character:
```
>>> print("Tab\tDelimited")
Tab     Delimited
>>> print("New\nLine")
New
Line
>>> print("Slash\\Character")
Slash\Character
>>> print("'Single' in Double")
'Single' in Double
>>> print('"Double" in Single')
"Double" in Single
>>> print("\"Double\" in Double")
"Double" in Double
```
 Strings in python is immutable and cannot be modified. They can be created with new version.

## Lecture: Numbers
Let's learn about some of the core data types in Python: the number types int and float.
Python Documentation For This Video

    numeric types (the int and float types): https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex

Numbers

There are 2 main types of numbers that we'll use in Python, int and float, and for the most part we won't be calling methods on number types and we will instead be using a variety of operators.
```
>>> 2 + 2 # Addition
4
>>> 10 - 4 # Subtraction
6
>>> 3 * 9 # Multiplication
27
>>> 5 / 3 # Division
1.66666666666667
>>> 5 // 3 # Floor division, always returns a number without a remainder
1
>>> 8 % 3 # Modulo division, returns the remainder
2
>>> 2 ** 3 # Exponent
8
```
If either of the numbers in a mathmatical operation in Python is a float then the other will be converted before carrying out the operation and the result will always be a float.
Converting Strings and Numbers

It's not uncommon for we to need to convert from one type to another when writing a script and Python provides built in functions for doing that with the built in types. For strings and numbers, we can use the str, int, and float functions to convert from one type to another (within reason).
```
>>> str(1.1)
'1.1'
>>> int("10")
10
>>> int(5.99999)
5
>>> float("5.6")
5.6
>>> float(5)
5.0
```
You'll run into issues trying to convert strings to other types if they aren't present in the string
```
>>> float("1.1 things")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: could not convert string to float: '1.1 things'
```
## Booleans and None
Python Documentation For This Video

    Booleans & None:
    Ref: https://docs.python.org/3/library/stdtypes.html#truth-value-testing

Booleans

Booleans represent "truthiness" and Python has two boolean constants: True and False.

Notice that these both start with capital letters. Later we will learn about comparisons operations, and those will often return either True or False.
Representing Nothingness with None

Most programming languages have a type that represents the lack of a value, and Python is no different. The constant used to represent nothingness in Python is None. None is a "falsy", and we'll often use it to represent when a variable has no value yet.

An interesting thing to note about None is that if you type None into your REPL there will be nothing printed to the screen. That's because None actually evaluates into nothing.

##  Working with Variables
Almost any script we write will need to have a way for us to hold on to information for use later on. That's where variables come into play.
Working with Variables

We can assign a value to a variable by using a single = and we don't need to (nor can we) specify the type of the variable.
```
>>> my_str = "This is a simple string"
```
Now we can print the value of that string by using my_var later on:
```
>>> print(my_str)
This is a simple string
```
Before, we talked about how we can't change a string because it's immutable. This is easier to see now that we have variables.
```
>>> my_str += " testing"
>>> my_str
'This is a simple string testing'
```
That didn't change the string — it reassigned the variable. The original string of "This is a simple string" was unchanged.

An important thing to realize is that the contents of a variable can be changed, and we don't need to maintain the same type.
```
>>> my_str = 1
>>> print(my_str)
1
```
Ideally, we wouldn't change the contents of a variable called my_str to be an int, but it is something that Python would let us do.

One last thing to remember is that if we assign a variable with another variable it will be assigned to the result of the variable and not whatever that varible points to later.
```
>>> my_str = 1
>>> my_int = my_str
>>> my_str = "testing"
>>> print(my_int)
1
>>> print(my_str)
testing
```
## Lists
In Python, there are a few different "sequence" types that we're going to work with, the most common of which being the list type.
Python Documentation For This Video

    Sequence Types
      https://docs.python.org/3/library/stdtypes.html#sequence-types-list-tuple-range
    Lists
      https://docs.python.org/3/library/stdtypes.html#list
Lists

A list is created in Python by using the square brackets ([, and ]) and separating the values by commas. Here's an example list:
```
>>> my_list = [1, 2, 3, 4, 5]
```
There's really not a limit to how long our list can be (there is, but it's very unlikely that we'll hit it while scripting)
Reading from Lists

To access an individual element of a list you can use the index and Python uses a zero based index system.
```
>>> my_list[0]
1
>>> my_list[2]
3
```
If we try to access an index that is too high (or too low) then we'll receive an error:
```
>>> my_list[5]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```
To make sure that we're not trying to get an index that is out of range, we can test the length using the len function (and then subtract 1)
```
>>> len(my_list)
5
```
Additionally, we can access subsections of a list by "slicing" it. We provide the starting index and the ending index (the object at that index won't be included).
```
>>> my_list[0:2]
[1, 2]
>>> my_list[1:]
[2, 3, 4, 5]
>>> my_list[:3]
[1, 2, 3]
>>> my_list[0::1]
[1, 2, 3, 4, 5]
>>> my_list[0::2]
[1, 3, 5]
```
> Step values in the slicing can be negative which will reverse the list. 

Modifying a List

Unlike strings which can't be modified (you can't change a character in a string), you can change a value in a list using the subscript equals operation:
```
>>> my_list[0] = "a"
>>> my_list
['a', 2, 3, 4, 5]
```
If we want to add to a list we can use the .append method. This is an example of a method that modifies the object that is calling the method:
```
>>> my_list.append(6)
>>> my_list.append(7)
>>> my_list
['a', 2, 3, 4, 5, 6, 7]
```
Lists can be added together (concatenated):
```
>>> my_list + [8, 9, 10]
['a', 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> my_list += [8, 9, 10]
>>> my_list
['a', 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
Items in lists can be set using slices also:
```
>>> my_list[1:3] = ['b', 'c']
>>> my_list
['a', 'b', 'c', 4, 5, 6, 7, 8, 9, 10]
# Replacing 2 sized slice with length 3 list inserts new element
my_list[3:5] = ['d', 'e', 'f']
print(my_list)
```
We can remove a section of a list by assigning an empty list to the slice:
```
>>> my_list = ['a', 'b', 'c', 'd', 5, 6, 7]
>>> my_list[4:] = []
>>> my_list
['a', 'b', 'c', 'd']
```
Removing items from a list based on value can be done using the .remove method:
```
>>> my_list.remove('b')
>>> my_list
['a', 'c', 'd']
```
Attempting to remove and item that isn't in the list will result in an error:
```
>>> my_list.remove('f')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: list.remove(x): x not in list
```
Items can also be removed from the end of a list using the pop method:
```
>>> my_list = ['a', 'c', 'd']
>>> my_list.pop()
'd'
>>> my_list
['a', 'c']
```
We can also use the pop method to remove items at a specific index:
```
>>> my_list.pop(0)
'a'
>>> my_list
['c']
>>> my_list.pop(1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: pop index out of range
>>> [].pop()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: pop from empty list
```
## Tuples & Ranges
The most common immutable sequence type that we're going to work with is going to be the tuple. We'll also look at the range type as an alternative to a sequential list.
Python Documentation For This Video

    Sequence Types
      https://docs.python.org/3/library/stdtypes.html#sequence-types-list-tuple-range
    Tuples
      https://docs.python.org/3/library/stdtypes.html#tuple
    Ranges
      https://docs.python.org/3/library/stdtypes.html#range

### Tuples

Tuples are a fixed width, immutable sequence type. We create tuples using parenthesis (( and )) and atleast one comma (,):
```
>>> point = (2.0, 3.0)
```
Since tuples are immutable, we don't have access to the same methods that we do on a list. We can use tuples in some operations like concatenation, but we can't change the original tuple that we created.
```
>>> point_3d = point + (4.0,)
>>> point_3d
(2.0, 3.0, 4.0)
```
One interesting characterist of tuples is that we can unpack them into multiple variables at the same time:
```
>>> x, y, z = point_3d
>>> x
2.0
>>> y
3.0
>>> z
4.0
```
The time you're most likely to see tuples will be when looking at a format string that's compatible with Python 2:
```
>>> print("My name is: %s %s" % ("Keith", "Thompson"))
```
### Ranges

Ranges are an immutable sequence type that defines a start, a stop, and a step value and then the values within are calculated as it is iteracted with. This allows for ranges to be used in place of sequential lists and while taking less memory and including more items.
```
>>> my_range = range(10)
>>> my_range
range(0, 10)
>>> list(my_range)
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> list(range(1, 14, 2))
[1, 3, 5, 7, 9, 11, 13]
```
Notice that the "stop" value is not included in the range, the values are all up-until the stop.

## Dictionaries (dicts)
Learn how to use dictionaries (the dict type) to hold onto key/value information in Python.
Python Documentation For This Video

    Dictionaries
        https://docs.python.org/3/library/stdtypes.html#mapping-types-dict
Dictionaries

Dictionaries are the main mapping type that we'll use in Python. This object is comparable to a Hash or "associative array" in other languages.

Things to note about dictionaries:

    Unlike Python 2 dictionaries, as of Python 3.6 keys are ordered in dictionaries. Will need OrderedDict if you want this to work on another version of Python.
    You can set the key to any IMMUTABLE type (no lists)
    Avoid using things other than simple objects as keys.
    Each key can only have one value (so don't have duplicates when creating a dict)

We create dictionary literals by using curly braces ({ and }), separating keys from values using colons (:), and separating key/value pairs using commas (,). Here's an example dictionary:
```
>>> ages = { 'kevin': 59, 'alex': 29, 'bob': 40 }
>>> ages
{'kevin': 59, 'alex': 29, 'bob': 40}
```
We can read a value from a dictionary by subscripting using the key:
```
>>> ages['kevin']
59
>>> ages['billy']
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'billy'
```
Keys can be added or changed using subscripting and assignment:
```
>>> ages['kayla'] = 21
>>> ages
{'kevin': 59, 'alex': 29, 'bob': 40, 'kayla': 21}
```
Items can be removed from a dictionary using the del statement or by using the pop method:
```
>>> del ages['kevin']
>>> ages
{'alex': 29, 'bob': 40, 'kayla': 21}
>>> del ages
>>> ages
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'ages' is not defined
>>> ages = { 'kevin': 59, 'alex': 29, 'bob': 40 }
>>> ages.pop('alex')
29
>>> ages
{'kevin': 59, 'bob': 40}
>>> {}.pop('billy')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'billy'
```
Since this is our second time running into a KeyError it's worth looking at a way to avoid these when we're attempting to read data from a dictionary. For that, we can use the get method:
```
>>> ages.get('kevin')
59
>>> ages.get('billy')
>>>
```
Now we're receiving None instead of raising an error when we try to get the value for a key that doesn't exist.

It's not uncommon to want to know what keys or values we have without caring about the pairings. For that situation we have the values and keys methods:
```
>>> ages = {'kevin': 59, 'bob': 40}
>>> ages.keys()
dict_keys(['kevin', 'bob'])
>>> list(ages.keys())
['kevin', 'bob']
>>> ages.values()
dict_values([59, 40])
>>> list(ages.values())
[59, 40]
```
Alternative Ways to Create a dict Using Keyword Arguments

There are a few other ways to create dictionaries that we might see, those being using the dict constructor with key/value arguments and a list of tuples:
```
>>> weights = dict(kevin=160, bob=240, kayla=135)
>>> weights
{'kevin': 160, 'bob': 240, 'kayla': 135}
>>> colors = dict([('kevin', 'blue'), ('bob', 'green'), ('kayla', 'red')])
>>> colors
{'kevin': 'blue', 'bob': 'green', 'kayla': 'red'}
```
## Conditionals and Comparisons
Scripts become most interesting when they do the right thing based on the inputs that we provide. To start building robust scripts, we need to understand how to make comparisons and use conditionals.
Python Documentation For This Video

    Comparisons
      https://docs.python.org/3/library/stdtypes.html#comparisons
    if/elif/else
      https://docs.python.org/3/tutorial/controlflow.html#if-statements

Comparisons

There are some standard comparison operators that we'll use that match pretty closely to those used in mathematical equations. Let's take a look at them:
```
>>> 1 < 2
True
>>> 0 > 2
False
>>> 2 == 1
False
>>> 2 != 1
True
>>> 3.0 >= 3.0
True
>>> 3.1 <= 3.0
False
```
If we try to make comparisons of types that don't match up, we will run into errors:
```
>>> 3.1 <= "this"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: '<=' not supported between instances of 'float' and 'str'
>>> 3 <= 3.1
True
>>> 1.1 == "1.1"
False
>>> 1.1 == float("1.1")
True
```
We can compare more than just numbers. Here's what it looks like when we compare strings:
```
>>> "this" == "this"
True
>>> "this" == "This"
False
>>> "b" > "a"
True
>>> "abc" < "b"
True
```
Notice that the string 'b' is considered greater than the strings 'a' and 'abc'. The characters are compared one at a time alphabetically to determine which is greater. This concept is used to sort strings alphabetically.
The in Check

We often get lists of information that we need to ensure contains (or doesn't contain) a specific item. To make this check in Python, we'll use the in and not in operations.
```
>>> 2 in [1, 2, 3]
True
>>> 4 in [1, 2, 3]
False
>>> 2 not in [1, 2, 3]
False
>>> 4 not in [1, 2, 3]
True
```
### if/elif/else

With a grasp on comparisons, we can now look at how we can run different pieces of logic based on the values that we're working with using conditionals. The keywords for conditionals in Python are if, elif, and else. Conditionals are the first language feature that we're using that requires us to utilize whitespace to separate our code blocks. We will always use indentation of 4 spaces. The basic shape of an if statement is this:

if CONDITION:
    pass

The CONDITION portion can be anything that evaluates to True or False, and if the value isn't explicitly a boolean then it will be converted to determine how to carry out proceed past the conditional (basically using the bool constructor).
```
>>> if True:
...     print("Was True")
...
Was True
>>> if False:
...     print("Was True")
...
>>>
```
To add an alternative code path, we'll use the else keyword, followed by a colon (:), and indenting the code underneath:
```
>>> if False:
...     print("Was True")
... else:
...     print("Was False")
...
Was False
```
In the even that we want to check multiple potential conditions we can use the elif CONDITION: statement. Here's a more robust example:
```
>>> name = "Kevin"
>>> if len(name) >= 6:
...     print("name is long")
... elif len(name) == 5:
...     print("name is 5 characters")
... elif len(name) >= 4:
...     print("name is 4 or more")
... else:
...     print("name is short")
...
name is 5 characters
```
Notice that we fell into the first elif statement's block and then the second elif block was never executed even though it was true. We can only exercise one branch in an if statement.
