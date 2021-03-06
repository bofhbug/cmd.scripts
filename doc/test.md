_This page tells about `test` script introducing more usability and flexibility to Windows batch script._

# Conditionals on steroids #




# DESCRIPTION #

This script extends conditionals in Windows batch scenarios. Once
included to your script it allows to construct the more powerful and
flexible conditionals implemented on _steroids_.

This script declares two _functions_ implementing conditional operators
for performing additional conditional expressions over files and
strings. These functions exit with a status of (0) or (1) depending on
the evaluations of an expression.

Expression could be in the standard form for the `IF` operator used in
batch scenarios. For the details of the syntax read the help page by
the `IF /?` command.

The second form came from the Bash's `test` builtin command that
extends capabilities of the standard operator in batches. In other
words, there are **steroids**.


# USAGE #

Run the following command to see this page

```
    test HELP
```

Execute the following command to embed the functionality into your script
"filename"

```
    test APPEND-TO filename
```

After that you can get all these features in your script as below:

```
    call :if -f "%COMSPEC%" && echo FILE
```

These functions would be available on the whole system wherever you were.
Put this script to some place on your system declared in the `%PATH%`
variable and call the functions as it is shown below:

```
    call test :if -f "%COMSPEC%" && echo FILE
```


## `if EXPR` / `unless EXPR` ##

Evaluates conditional expressions and returns the status of (0) if the
expression is true, otherwise (1). Returns the status of (2) if an
invalid arguments are given.


_UNLESS works similar to IF but the sense of the test is reversed._


To perform extended conditional operators this solution considers some
arguments (starting with the "`-`" symbol) as special operators. To avoid
confusion between those operators and normal arguments it's recommended
wrap the comparable arguments within double quotes. Operators must be
left unquoted anyway. So the following examples will work properly:

```
    copy nul equ
    call :if  -e  "equ"      && echo FILE EXISTS
    call :if "-e"  equ  "-e" && echo EQUAL STRINGS
```


## Standard `IF` operators ##

```
    IF [NOT] ERRORLEVEL number
    IF [NOT] string1==string2
    IF [NOT] EXIST filename

    IF CMDEXTVERSION number
    IF [NOT] DEFINED variable
    IF [/I] string1 compare-op string2
```

### `NOT` ###
Specifies that Windows should carry out the command only if the condition
is false.

### `ERRORLEVEL number` ###
Specifies a true condition if the last program run returned an exit code
equal to or greater than the number specified.

### `string1==string2` ###
Specifies a true condition if the specified text strings match.

### `EXIST filename` ###
Specifies a true condition if the specified filename exists.

### `CMDEXTVERSION number` ###
The CMDEXTVERSION conditional works just like ERRORLEVEL, except it
iscomparing against an internal version number associated with the Command
Extensions.

### `DEFINED variable` ###
The DEFINED conditional works just like EXIST except it takes an
environment variable name and returns true if the environment variable is
defined.

### `EQU` ###
Equal

### `NEQ` ###
Not equal

### `LSS` ###
Less than

### `LEQ` ###
Less than or equal

### `GTR` ###
Greater than

### `GEQ` ###
Greater than or equal

### `/I` ###
If specified, says to do case insensitive string compares. The `/I` switch
can also be used on the `string1==string2` form of `IF`. These comparisons
are generic, in that if both `string1` and `string2` are both comprised of
all numeric digits, then the strings are converted to numbers and a
numeric comparison is performed.


## Extended file operators ##

### `-a FILE` ###
True if file exists.

### `-b FILE` ###
True if file is drive.

### `-c FILE` ###
True if file is character device.

### `-d FILE` ###
True if file is a directory. Similar to `-attr d`.

### `-e FILE` ###
True if file exists.

### `-f FILE` ###
True if file exists and is a regular file.

### `-h FILE` ###
True if file is a link. Similar to `-attr l`.

### `-L FILE` ###
True if file is a link. Similar to `-attr l`.

### `-r FILE` ###
True if file is read only. Similar to `-attr r`.

### `-s FILE` ###
True if file exists and is not empty.

### `-w FILE` ###
True if the file is writable, i.e. not read only.

### `-x FILE` ###
True if the file is executable.

### `-attr ATTR FILE` ###
True if ATTR is set for FILE.

The following attributes can be recognized:
```
    Attribute                    Expansion
    FILE_ATTRIBUTE_DIRECTORY     d--------
    FILE_ATTRIBUTE_READONLY      -r-------
    FILE_ATTRIBUTE_ARCHIVE       --a------
    FILE_ATTRIBUTE_HIDDEN        ---h-----
    FILE_ATTRIBUTE_SYSTEM        ----s----
    FILE_ATTRIBUTE_COMPRESSED    -----c---
    FILE_ATTRIBUTE_OFFLINE       ------o--
    FILE_ATTRIBUTE_TEMPORARY     -------t-
    FILE_ATTRIBUTE_REPARSE_POINT --------l
    FILE_ATTRIBUTE_NORMAL        ---------
```

### `-path FILE` ###
True if FILE is listed in the PATH environment variable.


## More file operators ##

### `FILE1 -nt FILE2` ###
True if FILE1 is newer than FILE2 (according to modification time). This
operator is depending on the user-defined settings or locales, that means
that the result of this comparison cannot be considered as reliable.

### `FILE1 -ot FILE2` ###
True if FILE1 is older than FILE2 (according to modification time). This
operator is depending on the user-defined settings or locales, that means
that the result of this comparison cannot be considered as reliable.


## Extended string operators ##

### `-n STRING` ###
True if STRING is not empty.

### `-z STRING` ###
True if STRING is empty.


## More string operators ##

### `STACK -contains NEEDLE` ###
True if STACK contains NEEDLE.

### `STACK -starts NEEDLE` ###
True if STACK starts with NEEDLE.

### `STACK -ends NEEDLE` ###
True if STACK ends with NEEDLE.


# EXAMPLES #

`%COMSPEC%` is file
```
    call :if -f "%COMSPEC%" && echo FILE
```

`%COMSPEC%` is not directory
```
    call :unless -d "%COMSPEC%" && echo FILE
```

Do something if `%STR%` is not empty string
```
    call :if -n "%STR%" && echo NOTEMPTY
```

Do something if `%STR%` is empty string
```
    call :if -z "%STR%" && echo EMPTY
```

Do something depending on a file attribute
```
    call :if -d "%FILE%" && (
        echo DIR
    ) || ( call :if -f "%FILE%" ) && (
        echo FILE
    ) || (
        echo UNKNOWN
    )
```

Full test example
```
    set "f=%~1"
    set "a=%~a1"
    echo FILE: "%f%"
    echo ATTR: "%a%"
    call test :if -a "%f%" && echo -a
    call test :if -b "%f%" && echo -b
    call test :if -c "%f%" && echo -c
    call test :if -d "%f%" && echo -d
    call test :if -e "%f%" && echo -e
    call test :if -f "%f%" && echo -f
    call test :if -h "%f%" && echo -h
    call test :if -L "%f%" && echo -L
    call test :if -r "%f%" && echo -r
    call test :if -s "%f%" && echo -s
    call test :if -w "%f%" && echo -w
    call test :if -x "%f%" && echo -x
```


# LIMITATIONS #

Even though the steroids cover and unify many uncertain features of
batch files they have few limitations coming from the batches
themselves.

We can never be sure that arguments of scripts are completely checked.
Anytime a user can pass a string to an argument that will be executed
as never expected.

Sometimes we can lost some input characters in the arguments. For
example, it is true for `!` under the `enabledelayedexpansion` mode.


# REFERENCES #

  * [BatchLibrary or how to include batch files](http://www.dostips.com/forum/viewtopic.php?p=6475#p6475)
  * [Testing If a Drive or Directory Exists from a Batch File](http://support.microsoft.com/kb/65994)
  * [How to check if parameter is file (or directory)?](http://www.dostips.com/forum/viewtopic.php?f=3&t=2464)
  * [Elegant idea that inspired this work (Russian forum)](http://forum.script-coding.com/viewtopic.php?pid=55000#p55000)
  * [:strLen - returns the length of a string](http://www.dostips.com/DtTipsStringOperations.php#Function.strLen)
  * [The fastest method of the string length estimation (Russian forum)](http://forum.script-coding.com/viewtopic.php?pid=71000#p71000)
  * [Extended Attributes of a file](http://ss64.com/nt/syntax-args.html#attributes)


# COPYRIGHTS #

Copyright (c) 2013, 2014 Ildar Shaimordanov
