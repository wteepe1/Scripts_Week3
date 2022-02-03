# Shell Scripts and More Bash Commands

## Variables

You can create a variable and assign it a value using `=`

`myVariable=2`

= -assignment operator

Note that bash is sensitive to spaces! Don't leave any spaces before or after your equals sign when assigning a value to a variable.

You can print the value of a variable by using `echo` and prefacing the name of the variable with `$`

`echo $myVariable`

## Math in Bash

In general, bash isn't very good for mathematical operations, but it can be done in several ways. Probably the easiest is to wrap a mathematical expression in _double_ parentheses and precede it with a `$`, since you pretty much always want the _value_ of the mathematical result.

- `echo $(( 2 + 2 ))`
  - `myNum = $(( 2 + 2 ))`
- `echo $(( 3 * 6 ))`
- `echo $(( 20 / 3 ))`
- `echo $(( 20 % 3 ))`
    - gives us the remander in division

Compare the output of these last two lines? What's going on?

NOTE: bash can only handle _integers_ and not floating-point numbers (i.e., decimals)

Try this. What happens?

```
myVar=3
echo $myVar
((myVar++))
echo $myVar
((myVar++))
echo $myVar
```

What does the `++` operator do?
-adds one to the value of the variable (can be used to track how many times youve done something

## Downloading a file from the command line

An example data file is available here: [chiari.summary_statistics.csv](https://raw.githubusercontent.com/FoundCompBio-Spr22/Scripts_Week3/main/chiari.summary_statistics.csv)

While we could copy and paste the contents of this file into a new file using the graphical interface, this approach becomes tedious and difficult with lots of files or large files. Instead, we'd like to be able to download the file contents directly from the command line. To do this, we can use the `curl` command. To start, try executing this command

`curl https://raw.githubusercontent.com/FoundCompBio-Spr22/Scripts_Week3/main/chiari.summary_statistics.csv`

What happens? How could we save the contents that we're downloading to a file directly?
  -it displayed the contents of the file, but did not save it
  -Curl ... > ... saves it as a new file and allows you to name it

## Searching Within Files

Sometimes you may want to see if a certain text pattern exists in a file without viewing the entire file. In this case, the `grep` command is incredibly powerful. The basic syntax is

`grep <PATTERN> <PATH_TO_FILE>`

For instance, to search for the string `five` in the file `test.txt`, we could use

`grep five test.txt`

Take a look at the output. Note that the _entire_ line containing five is returned and not just the string itself.

Wildcards and additional symbols can be used to make searches general or more specific. For instance, using

`grep fi* test.txt`

will match any string that starts with `fi`.

`grep ^five test.txt`

will only look for the string `five` if it is at the very beginning of a line.

`grep five$ test.txt`

will only look for the string `five` if it is at the end of a line.

There are many other possible patterns that can be used with `grep`. If you search online for grep cheat sheets, you can find a lot more information.
-In grep ".*" is equal to "*"

## Redirecting Output Streams

In the examples above, the output of the display commands (like `cat`, `head`, and `tail`) were all sent to the Terminal screen. However, there are many times when it is preferable to send this output to a file or even as the input to another command!

To send the output of any command to be stored in a file, you can use the redirection operators `>` and `>>`. These commands differ in whether they overwrite or append to a file. It's always safter to use `>>` if you're worried about losing the contents of an existing file. Note that if you overwrite a file's contents, there's __no__ way to get it back.

As one example, here's a way to send the contents of all text (`.txt`) files into a new text file called `allContents.txt`.

`cat *.txt >> allContents.txt`

You can also send the output of `head` or `tail` to a file in the same way

`head -n 20 test.txt >> testHeader.txt`

Sometimes the output of one command is also convenient to use as the input to another command. For instance, you could extract the first few lines of a file with `head` and then target the last few of these header lines with `tail`. To do this, we use a special type of redirection called piping. The pipe symbol is `|`. Here's one example of piping with `head` and `tail`:

`head -n 10 test.txt | tail -n 5`

Note that, in this case, `tail` does not need a filename as an argument. It is taking its input from the pipe.

As usual, this output is printed to the screen by default. But it can also be redirected to a file, even after piping:

`head -n 10 test.txt | tail -n 5 >> myLines.txt`

Sometimes we need to use or store the output from a (series of) bash command(s), but a pipe won't accomplish what we need. One of these cases is storing output from commands in a variable. In this case, we can wrap the bash commands with backticks to indicate that they should be executed first, and the resulting value should be stored in the variable

```
myVariable=`ls | head -n1`
```

## Parsing File Contents

Another very powerful command is called `awk`, which can do lots of different forms of text parsing. For the purposes of this course, we will focus on using `awk` to extract individual columns (generally separated by spaces or tabs) from a file. The syntax we will use looks like this

`awk '{print $1}' test.txt`

This will print out the first column. To print the third column, we would use

`awk '{print $3}' test.txt`

To print both the first and third columns, we could do this

`awk '{print $1,$3}' test.txt`

## Command line find-and-replace

You'll often need to clean up the contents of data files before doing additional analyses. In this case, we'd like to replace commas with spaces. To do this from the command line, we can use a powerful tool called `sed`. `sed`, which is short for "stream editor", can do many different things, but we'll just use it for simple find-and-replace for now. Try executing this

`sed 's/,/ /g' chiari.summary_statistics.csv`

What do you see? What's different compared to the original contents of the file?

What about when you run this?

`sed -i "_backup" 's/,/ /g' chiari.summary_statistics.csv`

Note that this syntax will stay the same for any find and replace operation that we want to do. The only thing that will change is the text to find and replace (between the slashes).

```
Practice Exercise 1

(1) Create a new variable (e.g., myVar) and assign a numeric value to this variable.
(2) Print the value of the variable to the screen.
(3) Use the increment operator to add 1 to the variable's value.
(4) Use history, a pipe, tail, and >> to extract the last myVar lines from your command history and store them in a new file (recentCommands.txt).
(5) Open your new file with less and scroll through to examine the contents.
(6) Use sed to do a find-and-replace operation to change all instances of "echo" in the file to "BIOL4800".
(7) Use grep to extract any lines from the file that start with "ls".
```

## Introduction to Shell Scripts

_What is a shell/bash script?_

Fundamentally, a bash script is just a file containing a series of bash commands. Scripts are plain text files, but the text in this file is special.

The first line of a script tells the computer in which language (i.e., shell) we're writing our script. This line starts with `#!` - also known as a shebang. The shebang tells Terminal that we're about to indicate which language we're going to use. Follow the shebang with the path to the shell that you'd like to use. Yes, the shell itself is a program!

`#! /bin/bash`

Let's start by creating your first script - `myScript.sh`

- `nano myScript.sh`
- Add the shebang line
- Add two commands in the body of the file
  - `echo "Hello, "$USER"!"`
  - `echo "I'M A SCRIPT AND I WORK!"`

To run the script, make sure it's located in your current working directory. Then, type

`./myScript.sh`

Did it run? If not, why not? How can you fix the problem?

_Command-line Arguments_

Scripts can be written to use command-line arguments. To access the arguments from inside the script, bash reserves the special variables `$1`, `$2`, `$3`, ...

For practice, go back to `myScript.sh` that we created earlier and change `$USER` to `$1`. Now, run it by typing `myScript.sh <YOUR_NAME>`

```
Practice Exercise 2 (Math Script)

(1) Create a new script file. Be sure to add the shebang line and set permissions properly to execute.
(2) Allow your script to accept two command-line arguments that are numbers.
(3) Inside the script, multiply the two numbers and then subtract the second number from the product
(4) Print the result to the screen.
```

## If...Else

One of the other very important programming concepts is known as "flow control". Basically, this just means doing different things depending on current conditions. There are several ways to accomplish flow control, but probably the most common is the `if...else` statement, which looks like this:

```
if <TEST_SOMETHING>
then
  <DO_THING_1>
else
  <DO_THING_2>
fi
```

There are lots of different ways to compare values, depending on what type of values you're working with. Here is a page that lists several options: [Bash comparison operators](http://tldp.org/LDP/abs/html/comparison-ops.html). Here's one example:

```
# Defining value of numeric variable
a=2
        
# if...else to see if value of number is at least 3
if [ $a -lt 3 ]  # Note: this could also be ((a < 3))
then
  echo "$a is less than 3."
else
  echo "$a is NOT less than 3."
fi
```

Here's an example of combining backticks, command-line arguments, and an if...else statement to write a script that tests if a command-line argument has a certain number of characters.

```
# Recording length of word provided on command line
# What's going on with the backticks (``) here?
myWordLength=`echo -n $1 | wc -m`

# test if word is at least 5 characters
if [ $myWordLength -lt 5 ]
then
  echo "$1 is shorter than 5 characters."
else
  echo "$1 is at least 5 characters in length."
fi
```

If...else statements can also be nested inside one another.

```
if [ $myWordLength -le 7 ]
then
    if [ $myWordLength -ge 3 ]
    then
        echo "Good."
    else
        echo "Not good."
    fi
else
    echo "Not good."
fi
```

```
Practice Exercise 3 (If...Else)

(1) Start with the script you wrote above to practice math
(2) Now add two if...else statements to check that:
  - The first number is between 3 and 7
  - The second number is between 10 and 14
(3) If either of these conditions are not met, have your script print an error message to the screen.
```

### If...Elif...Else

Sometimes you may want to evaluate a series of conditions consecutively. In this case, you can use a variation on an `if...else` statement that has an extra `else if` in the middle. In these cases, the `else if` is abbreviated as `elif` and the structure could look something like this

```
if (( $1 < 2 ))
then
    echo "less than two"
elif (( $1 < 5 ))
then
    echo "between two and four"
else
    echo "greater than four"
fi
```

How is this code different than what we saw above? If we put this in a script and passed a command-line argument of `1`, what would we get? What if we passed a `4`?

You can also string together as many `elif` tests in a row as you want.

```
if (( $1 < 2 ))
then
    echo "less than two"
elif (( $1 < 5 ))
then
    echo "between two and four"
elif (( $1 < 8 ))
then
    echo "between five and seven"
else
    echo "greater than seven"
fi
```
