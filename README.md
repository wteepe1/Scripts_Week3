# Shell Scripts and More Bash Commands

## Variables

You can create a variable and assign it a value using `=`

`myVariable=2`

Note that bash is sensitive to spaces! Don't leave any spaces before or after your equals sign when assigning a value to a variable.

You can print the value of a variable by using `echo` and prefacing the name of the variable with `$`

`echo $myVariable`

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

- Command-line Arguments
    - To access the argument from inside the script, bash reserves the special variables `$1`, `$2`, `$3`, ...
    - For practice, go back to `myScript.sh` that we created earlier and change `$USER` to `$1`. Now, run it by typing `myScript.sh <YOUR_NAME>`

