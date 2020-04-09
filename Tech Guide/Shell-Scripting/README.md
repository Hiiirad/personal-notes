# Shell Scripting

> The minimum knowledge of your Linux and Unix systems should be LPIC 1 to understand shell scripting.

> This note is for SysAdmins or DevOps Engineers, so most of the commands of this course run on Redhat-based distributions such as Redhat, Centos, etc.

> I used bash as a default shell for these commands. You may have some different outputs based on your default shell.

## Part 01 (Linux Review)

Shell Scripting is an interpreter and cross-platform programming language.

- Find all available shells in your system:
  ```bash
  cat /etc/shells
  ```
- Default shell after login:
  ```bash
  echo $SHELL
  ```
- See all user's default shell:
  ```bash
  cat /etc/passwd
  ```
- Change shell permanently:
  ```bash
  chsh -s SHELL_PATH
  chsh -s /bin/bash
  ```
- Predefined Variables:
  ```bash
  echo $# # Tell you number of command line arguments supplied
  echo $0 # Tell you shell name
  echo $1 # First argument to the script
  echo $2 # Second argument to the script, and so on...
  echo $$ # PID of our shell
  echo $! # PID of the last background process
  echo $- # Current shell status
  echo $? # Exit status of last command:
          # 0: Successful exit
          # 1-255: Error on exit
          # 126: Not executable
          # 127: Command not found
          # 130: Command terminated with ^C or Ctrl+C
          # Exit code more than 255 returns an exit code modulo 256
          # exit 3809 = exit 225
  ```
- See list of processes from other users:
  - Unix-Style:
    ```bash
    ps -ef
    ```
  - BSD-Style:
    ```bash
    ps -aux
    ```
  - Process State:<br>
  ![Process State](Images/Process_State.png)

- You can compile shell script to C using `shc` command.
- Revert your shell to the previous shell using `exit`.
- Shell level:
  ```bash
  echo $SHLVL
  ```
- Set execute permission to script:
  ```bash
  chmod PERMISSION SCRIPT_NAME
  chmod u+rx SCRIPT
  chmod 700 SCRIPT
  ```
- Execute a script:
  ```bash
  /PATH/.../SCRIPT # Full path
  ./SCRIPT         # Relative path
  bash SCRIPT      # Shell tells OS to fork me and run the program on that.
  . SCRIPT         # Shell tells OS to don't fork me! And doesn't need permission to run!
  source SCRIPT

  (cat /etc/passwd; sleep 500)   # Current shell execute it
  (cat /etc/passwd; sleep 500) & # This command runs in background
  {cat /etc/passwd; sleep 500; } # This command runs in subshell
  ```
- You can bypass aliased command with these methods:
  - The full path of the command: `/bin/ls`
  - Command substitution: `$(which ls)`
  - The command builtin: `command ls`
  - Using Double quotation: `"ls"`
  - Using single quotation: `'ls'`
  - Using backslash character: `\ls`
- Understand internal (built-in) or external command using `type COMMAND`.
  - If the command is builtin, you see an output like this: `COMMAND is a shell builtin.`
  - If the command isn't builtin, you can see 2 different output based on what you did:
    1. If you run `type COMMAND` before executing COMMAND, you see an output like this: `COMMAND is COMMAND_PATH`
    2. If you run `type COMMAND` after executing COMMAND, you see an output like this: `COMMAND is hashed (COMMAND_PATH)`
  - **Hash** means you just execute an external command, and you can see them by simply run `hash` command.
  - When an external command is executed, shell tells OS to make a fork from the shell and then run your external command in it and then exits. You can see Virtual Memory Statistics using this command: `vmstat -s`
  - Fork Diagram:<br>
    ![Fork Diagram](Images/Fork_Diagram.png)
  - Execute `ls` command diagram:<br>
    ![Exec ls](Images/exec_ls.png)

- You can see the Magic Number of a file here: `less /usr/share/magic`
  - Windows executable file's magic number starts with **MZ**
  - Other OS executable file's magic number starts with **ELF**
  - ELF: Executable Library Format
  - You can see magic number of a file using this command: `od -c FILE | less` or `file FILE`
- A script must have at least Read permission and Execute permission. Something like this:
  ```bash
  chmod u+rx SCRIPT
  ```
- It's recommended to put comment(s) at the beginning of the script.
  ```bash
  # Script name: SAMPLE
  # Author: FIRST_NAME LAST_NAME
  # Date of Creation: YYYY-MM-DD
  # Version: MAJOR.MINOR.BUG
  # Last Modified on: YYYY-MM-DD
  # Description: THIS IS A SAMPLE DOCUMENTATION
  echo THIS IS A SAMPLE SCRIPT
  ```
- Shell startup file: (Executed for login shell)
  - `/etc/profile` : System-wide default environment settings.
  - `~/.bash_profile` : User-specific bash environment default settings.
- Initialization Files: (Executed for login and interactive shell)
  - `/etc/bashrc` : System-wide functions and aliases for bash
  - `~/.bashrc` : User-specific initialization files
- Shell input and outputs:
  - Standard input = STDIN = 0
  - Standard output = STDOUT = 1
  - Standard error = STDERR = 2
- Shell variables can contain different sizes based on your system. You can see your system's limitations with `xargs --show-limits` command.
- The first prompt you see when you open a shell called primary shell, 1 which you can see with `echo $PS1` command and the secondary shell that you see called primary shell 2, which you can see with `echo $PS2`. (You can see PS2 when you use inline input redirection + You can change them in your shell configuration file)
- Signals:
  - Signal is a notification, a message sent by either operating system or some application to our program. Signals are a mechanism for one-way asynchronous notifications. A signal may be sent from the kernel to a process, from a process to another process, or from a process to itself. Signal typically alert a process to some event, such as a segmentation fault, or the user pressing Ctrl-C. Linux kernel implements about 30 signals. Each signal identified by a number, from 1 to 31. Signals don't carry any argument and their names are mostly self explanatory.
  - Signals are raised by error conditions, and they are generated by the shell and terminal handlers to cause interrupts and can also be sent from one process to another to pass information or to modify the behavior.
  - Signals can be:
    1. Raised
    2. Caught
    3. Acted Upon
    4. Ignored
  - If a process receives signals such as `SIGFPE`, `SIGKILL`, etc., the process will be terminated immediately, and a core dump file is created. The core file is an image of the process, and we can use it to debug.
  - List of all signals:
    |Signal|Name|Description|
    |------|----|-----------|
    |SIGHUP|1|Hangup (POSIX)|
    |SIGINT|2|Terminal interrupt (ANSI)|
    |SIGQUIT|3|Terminal quit (POSIX)|
    |SIGILL|4|Illegal instruction (ANSI)|
    |SIGTRAP|5|Trace trap (POSIX)|
    |SIGIOT|6|IOT Trap (4.2 BSD)|
    |SIGBUS|7|BUS error (4.2 BSD)|
    |SIGFPE|8|Floating point exception (ANSI) |
    |SIGKILL|9|Kill (can't be caught or ignored) (POSIX)|
    |SIGUSR1|10|User defined signal 1 (POSIX)|
    |SIGSEGV|11|Invalid memory segment access (ANSI)|
    |SIGUSR2|12|User defined signal 2 (POSIX)|
    |SIGPIPE|13|Write on a pipe with no reader, Broken pipe (POSIX)|
    |SIGALRM|14|Alarm clock (POSIX)|
    |SIGTERM|15|Termination (ANSI)|
    |SIGSTKFLT|16|Stack fault|
    |SIGCHLD|17|Child process has stopped or exited, changed (POSIX)|
    |SIGCONTv|18|Continue executing, if stopped (POSIX)|
    |SIGSTOP|19|Stop executing (can't be caught or ignored) (POSIX)|
    |SIGTSTP|20|Terminal stop signal (POSIX)|
    |SIGTTIN|21|Background process trying to read, from TTY (POSIX)|
    |SIGTTOU|22|Background process trying to write, to TTY (POSIX)|
    |SIGURG|23|Urgent condition on socket (4.2 BSD)|
    |SIGXCPU|24|CPU limit exceeded (4.2 BSD)|
    |SIGXFSZ|25|File size limit exceeded (4.2 BSD)|
    |SIGVTALRM|26|Virtual alarm clock (4.2 BSD)|
    |SIGPROF|27|Profiling alarm clock (4.2 BSD)|
    |SIGWINCH|28|Window size change (4.3 BSD, Sun)|
    |SIGIO|29|I/O now possible (4.2 BSD)|
    |SIGPWR|30|Power failure restart (System V)|

## Part 02 (Linux Commands)

- `grep` : Searches for PATTERN in each FILE (Highly recommended that you read all manual page of this command)
  - `-E` or `--extended-regexp` or `egrep`
  - `-F` or `--fixed-strings` or `fgrep`
  - `-i` or `--ignore-case`
  - `-l` or `--files-with-matches`
  - `-r` or `--recursive` or `rgrep`
  - `-v` or `--invert-case`
- `tee` : Read from STDIN and write to STDOUT and files
    ```bash
    echo hello | tee file1.txt file2.txt
    ```
  - Useful options:
    - `-a`, `--append` : Append to the given FILEs, do not overwrite
    - `-i`, `--ignore-interrupts` : Ignore interrupt signals
    - `--output-error=MODE` : Set behavior on write error. See MODE below:
      - warn: Diagnose errors writing to any output
      - warn-nopipe: Diagnose errors writing to any output not a pipe
      - exit: Exit on error writing to any output
      - exit-nopipe: Exit on error writing to any output not a pipe
- `nl` : Write each FILE to standard output, with line numbers added
    ```bash
    nl /etc/passwd
    ```
- `find` : Search for files in a directory hierarchy (Highly recommended that you read all manual page of this command or at least see examples of manual page)
    ```bash
    find LOCATION_OF_START ARGUMENT(S)
    find /test -name "*.txt"
    ```
  - Useful options:
    - You can only use one of these; otherwise, each overrides the others. The last one appearing on the command line takes effect. Since `-P` is the default, the `-P` option should be considered to be in effect unless either `-H` or `-L` is specified.
      - `-P` : Never follow symbolic links
      - `-L` : Follow symbolic links.
      - `-H` : Do not follow symbolic links, except while processing the command line arguments.
    - `-empty` : File is empty and is either a regular file or a directory.
    - `-executable` : Matches files which are executable and directories which are searchable (in a file name resolution sense).
    - `-gid N` : File's numeric group ID is N.
    - `-name` : Base of the file name (the path with the leading directories removed) matches shell pattern.
    - `-iname` : Like -name, but the match is case insensitive.
    - `-iregex` : Like -regex, but the match is case insensitive.
    - `-regex` : File name matches the regular expression pattern.
    - `-size N` : File uses N units of space, rounding up. The following suffixes can be used:
      - `b` : for 512-byte blocks (this is the default if no suffix is used)
      - `c` : for bytes
      - `w` : for two-byte words
      - `k` : for Kibibytes (KiB, units of 1024 bytes)
      - `M` : for Mebibytes (MiB, units of 1024 * 1024 = 1048576 bytes)
      - `G` : for Gibibytes (GiB, units of 1024 * 1024 * 1024 = 1073741824 bytes)
      - The + and - prefixes signify greater than and less than, as usual; i.e., the exact size of N units does not match.  Bear in mind that the size is rounded up to the next unit. Therefore -size -1M is  not  equivalent to -size -1048576c. The former only matches empty files, the latter matches files from 0 to 1,048,575 bytes.
    - `-type C` : File is of type C (To search for more than one type at once, you can supply the combined list of type letters separated by a comma)
      - `b` : block (buffered) special
      - `c` : character (unbuffered) special
      - `d` : directory
      - `p` : named pipe (FIFO)
      - `f` : regular file
      - `l` : symbolic link; this is never true if the `-L` option or the `-follow` option is in effect, unless the symbolic link is broken.  If you want to search for symbolic links when `-L` is in effect, use `-xtype`.
      - `s` : socket
      - `D` : door (Solaris)
      - To search for more than one type at once, you can supply the combined list of type letters separated by a comma or `,`
    - `-uid N` : File's numeric user ID is N.
    - `-user uname` : File is owned by user uname (numeric user ID allowed).
- `locate` : Find files by name
  - `-b` or `--basename` : Match only the base name against the specified patterns.  This is the opposite of `--wholename`.
  - `-c` or `--count` : Instead of writing file names on standard output, write the number of matching entries only.
  - `-i` or `--ignore-case` : Ignore case distinctions when matching patterns.
  - `-p` or `--ignore-spaces` : Ignore punctuation and spaces when matching patterns.
  - `-q` or `--quiet` : Write no messages about errors encountered while reading and processing databases.
  - `--regex` : Interpret all PATTERNs as extended regexps.
  - `-w` or `--wholename` : Match only the whole path name against the specified patterns. This is the default behavior. The opposite can be specified using `--basename`.
- `pushd` and `popd` : The `pushd` command saves the current working directory in memory so it can be returned to at any time, optionally changing to a new directory. The `popd` command returns to the path at the top of the directory stack.
  ```bash
  cd /tmp
  pushd /var/log
  # Output: /var/log /tmp
  cd ..
  pwd
  # Output: /var
  popd
  # Output: /tmp
  ```
- `script` : 
  - Script command can be used to make a record of everything that appears on the terminal.
  - Once started, all terminal outputs will also be copied into the *scriptfile* until a `^D` or `exit` is received.
    - `exit` : Exit command tells shell that I'm leaving/closing this connection.
    - `^D` : Control+D from keyboard device driver tells OS to break/close the connection.
      - You can see a list of them using `stty -a`.
      - Stty (Set TeleTYpe) is a command that changes keyboard and monitor commands. For example, it can change rows and columns of monitor, speed of keyboard, etc.
  - Where filename is optional and if not specified it is assumed to be **typescript**.

## Part 03 (Shell Script Basics)

- If you want to run multiple commands in one line, you must separate commands with semicolon or `;`
  ```bash
  date ; whoami ; echo Hello World!
  ```
- You can comment a line with `#` which has different names. (Sharp, Number, Pound, Hash or Hashtag)
- When creating a shell script file, you must specify the shell you are using only in the **first line** of file. It tells the OS what is the correct interpreter. The `#!` before shell path called **Shebang** and the format for this is:
  ```bash
  #!SHELL_PATH
  #!/bin/bash
  #!/bin/sh
  ```
- It's recommended that you put `.sh` extension at the end of your script file.
- Values are assigned to user variables using an equal sign. **No
spaces** can appear between the variable, the equal sign, and the
value.
- The shell script automatically determines the data type used for
the variable value.

## Part 04 (Variables)

- Rules of variables:
  - Variables are **case-sensitive**.
  - Variables cannot contain spaces.
  - Variables cannot start with a number.
  - Variables cannot contain special characters.
  - No spaces should be on either side of `=`
  - It's recommended that you set your variables in **UPPERCASE** letters.
  - If you want to create a variable and variable was already defined, the old value is overwritten with the new value.
- Using variable value preceded by a dollar sign or `$`
  ```bash
  VAR1=27
  VAR2=$VAR1
  ```
- The backtick or **\`** allows you to assign the output of a shell command
to a variable. You must surround the entire command line command with
backtick characters:
  ```bash
  TODAY=`date`
  echo $TODAY
  # or
  echo `date`
  ```
- There are two types of environment variables in the bash shell:
  - **Global/Environmental Variable**: Global variables, which are called environment variables, are visible from the shell session, and any child processes that the shell spawns.
  - **Local Variable**: Local variables are only available in the shell that creates them. This makes global environment variables useful in applications that spawn child processes that require information from the parent process.
- Environmental variables are defined in `/etc/profile`, `/etc/profile.d`, and `~/.bash_profile`. These files are the initialization files, and they are read by the shell when the bash shell invokes. When a login shell exits, bash reads `~/.bash_logout`.
- After login into the system, a copy of the shell is given to the user.
- The environment is maintained all the time until the user logs off.
- You can see all variables (Global + Local) with `set` command, and other options of it with `help set` command.
- You can see Global variables with `env` or `printenv` command.
- The method used to create a global environment variable is to create a local environment variable, then export it to the global environment. This is done by using the `export` command:
  ```bash
  VAR1=test
  export VAR1
  # Export command in some shells is different.
  # BSD & TCSH: setenv
  ```
- Exported variables and their values are copied into a subshell's environment, where they may be accessed and changed. However, such changes have no effect on the variables in the parent shell.
- A subshell is a new shell that is executed by the login shell to run the desired program.
- A subshell does not know local variables that were assigned values by the parent shell.
- The subshell cannot change the value of a variable in the parent shell.
- You can remove variables by using `unset` command.
  ```bash
  VAR1=testing
  unset VAR1
  echo VAR1   # No output
  ```
  - When referencing the environment variable in the unset command, remember **not to use the dollar sign or `$` before variable name**.
  - If you’re in a child process and unset a global environment variable, it only applies to the child process. The global environment variable is still available in the parent process.
- Single Quote vs Double Quote:
  - Using double quotes to show a string of characters allow any variables in the quotes to be resolved.
  ```bash
  VAR1=test
  echo "The value of var1 is $VAR1"
  # OUTPUT: The value of var1 is test
  echo 'The value of var1 is $VAR1'
  # OUTPUT: The value of var1 is $VAR1
  ```
- `readonly` : Use the readonly command to make variables and functions readonly. You cannot change the value of variables.
  ```bash
  readonly VAR
  readonly VAR=value
  
  VAR=new_value
  # Output: -bash: unset: VAR: cannot unset: readonly variable
  unset VAR
  # Output: -bash: unset: VAR: cannot unset: readonly variable
  ```
- 

## Part 05 (Special Characters)

There is a character called backslash or `\` or escape character that won't let shell to interpret that special character. For example:

```bash
#!/bin/bash
echo $HOME
# Output: /home/hirad
echo \$HOME
# Output: $HOME
echo "Hello World"
# Output: Hello World
echo \"Hello World\"
# Output: "Hello World"
```

Wildcards:
- `*` : Matches any string or group of characters
- `?` : Matches any single character
- `[]` : Matches any one of the enclosed characters
- `!` or `^` : Not
  ```bash
  ls a*
  ls u*.c
  ls a*b*c

  ls a?b
  ls ????
  ls x??y

  ls [abc]*
  ls [0-9]?[k-z]

  ls /bin/[!a-o]
  ls /bin/[^p-z]
  ls /bin/[!c-e]??
  ```

## Part 06 (Redirection)

- Output
  - `>` : Creating a new file or **overwriting** the existing file
    ```bash
    COMMAND > OUTPUT_FILE
    date > output.txt
    ```
  - `>>` : Creating a new file or **appending** to the existing file
    ```bash
    COMMAND >> OUTPUT_FILE
    who >> output.txt
    ```
- Input
  - `<` : Input redirection takes the content of a file and redirects it to a command.
    ```bash
    COMMAND < INPUT_FILE
    wc -l < /etc/passwd
    cat < /etc/services
    ```
  - `<<` : The inline input redirection method allows you to specify the data for input redirection on the command line instead of in a file. Besides this symbol, you must specify a text marker that delineates the beginning and end of the data used for input.
    ```bash
    COMMAND << MARKER
    DATA
    MARKER

    wc << EOF
    Hello
    line 2
    good-bye
    EOF
    ```
- Example (Write a script which uses inline input redirection to append some lines a an existing file in your home directory):
  ```bash
  #!/bin/bash
  cat >> TESTFILE << EOF
  Hello $USER
  We're learning shell scripting
  let's get some advance stuff
  see you later
  EOF
  ```
- Redirections with STDIN, STDOUT, and STDERR:
  - `<` : Redirects STDIN (0) from file
  - `>` : Redirects STDOUT (1) to file
  - `>>` : Append STDOUT to file
  - `<<` : Here-Document
  - `&>` : Redirects STDERR (2)
- Error Redirection:
  ```bash
  COMMAND 2> FILE
  date alex 2> output.txt
  # output.txt : date: invalid date 'alex'

  touch a
  date < a
  # Output: NORMAL DATE OUTPUT

  date < bob
  # Output: -bash : bob: No such file or directory

  # STDOUT goes where STDERR goes
  ls AA BB &> CC
  ```
- You can use `printf` to write something by using redirects. This command write/overwrite **NOTHING** to FILENAME. `printf "" > FILENAME`
- You can also use `> FILENAME` command to redirect **NOTHING** to the FILENAME. (This command doesn't always apply in some shells.)
- You can redirect STDOUT and STDERR in two different places with a command like this:
  ```bash
  COMMAND 2> ERROR_FILE.txt
  # Errors are going to write in a file.

  COMMAND > OUTPUT.txt 2> ERROR_FILE.txt
  # We don't have any output on the screen, because we write it in a file.
  ```
- It's a wise move to redirect your unessential outputs/errors to `/dev/null`
- Pipe or `|`
  - Redirect one program's output into another program's input.
  - A pipe (Unnamed pipe) is a way to connect the output of one program to the input of
another program **without any temporary file**.
  - Examples:
    ```bash
    #!/bin/bash
    who | less
    who | sort > user_list.txt
    who | wc -l
    ls -l | wc -l
    who | grep PATTERN
    ```

## Part 07 (Mathematics)

There are three different ways to perform mathematical operations in your shell scripts:

1. `expr` (NOT RECOMMENDED)
   - Pay attention to the space between operands and operator.
    ```bash
    expr 1 + 4
    # Output: 5
    expr 2+3
    # Output: 2+3
    ```
   - Operands for this command: `+  -  *  /  %  <  <=  =  !=  >=  >`
   - `expr` command only supports integer numbers. It doesn't support float numbers.
    ```bash
    expr 5 / 2
    # Output: 2
    ```
   - When you use these operands, you should escape these characters.
    ```bash
    expr 5 * 2
    # Output: expr: syntax error
    expr 5 \* 2
    # Output: 10
    ```
   - Example:
    ```bash
    #!/bin/bash
    VAR1=10
    VAR2=20
    VAR3=`expr $VAR2 / $VAR1`
    echo The result is $VAR3
    # Output: The result is 2
    ```
2. Brackets [ ]
   - In bash, when assigning a mathematical value to a variable, you can enclose the mathematical equation using a dollar sign and square brackets `$[operation]`
    ```bash
    VAR1=$[1+5]
    echo $VAR1
    # Output: 6
    VAR2=$[$VAR1*2]
    echo $VAR2
    # Output: 12
    ```
   - You can't calculate floating numbers in this solution, either.
- We don't usually need a floating number in shell scripting, because system administrators and DevOps Engineers don't need it for managing system.
3. Bash Calculator or `bc`
   - There's another way to calculate floating numbers. By using `bc` command. You can exit the program with `quit`. `bc` can recognize:
     - Numbers (integer and floating numbers)
     - Variables (simple variables and arrays)
     - Comments (Single line comment starts with `#`, or multiline comments start with `/*` and ends with `*/`)
     - Expressions
     - Programming statements (such as if-then statements)
     - Functions
   - The floating-point arithmetic is controlled by a built-in variable called **scale**. You must set this value to the desired number of decimal places you want in your answers; otherwise, the default value of scale is zero, and `bc` calculates only integer numbers.
   - Using `bc` in scripts:
    ```bash
    VARIABLE=`echo "OPTIONS; EXPRESSION" | bc`
    VAR1=20
    VAR2=`echo "scale=4;$VAR1/3" | bc`
    echo $VAR2
    # Output: 6.6666
    ```
   - If you have more than just a couple of calculations, you can use input redirection, allowing you to redirect a file to the bc command for processing. You just need to make a list of command and then `bc < COMMAND_FILE` or `cat COMMAND_FILE | bc`
   - Instead of using a file for redirection, you can use the inline input redirection method, which allows you to redirect data directly from the command line. In the shell script, you can assign the output to a variable. This looks like this:
    ```bash
    variable=`bc << EOF
    options
    statements
    expressions
    EOF
    `
    ```
   - Remember that the backtick characters are still needed to assign the output of the `bc` command to the variable.

## Part 08 (Conditional Tasks)

- **if/then**
  - If the exit code of our command is zero (0), the command and command(s) under *then* will execute; otherwise, command(s) under *then* will ignore.
  - Structure:
    ```bash
    #!/bin/bash
    if COMMAND
    then
      COMMAND(S)
    fi
    ```
  - By putting a semicolon at the end of the command to evaluate, you can include the then statement on the same line.
    ```bash
    #!/bin/bash
    if COMMAND; then
      COMMAND(S)
    fi
    ```
- **if/then/else**
  - If the command in the if statement line returns with an exit status code of zero, the command(s) listed in the *then* section are executed, just as in a normal if-then statement. If the command in the if statement line returns a non-zero exit status code, the bash shell executes the commands in the else section.
    ```bash
    #!/bin/bash
    if COMMAND
    then
      COMMAND(S)
    else
      COMMAND(S)
    fi
    ```
- **Nested if**
  - Sometimes you must check for several situations in your script code. Instead of having to write separate if-then statements, you can use an alternative version of the else section, called *elif*. The *elif* continues an else section with another if-then statement.
    ```bash
    #!/bin/bash
    if COMMAND1
    then
      COMMAND(S)
    elif COMMAND2
    then
      MORE_COMMAND(S)
    fi
    ```
- **test**
  - To evaluate any condition other than the exit status code of a command, we need to use test command. The test command provides a way to test different conditions in an if-then statement. If the condition listed in the test command evaluates to true, the test command exits with a zero exit status code.
    ```bash
    #!/bin/bash
    if test CONDITION
    then
      COMMAND(S)
    fi
    ```
  - Test command has special structure to follow. There have to be a blank between operands and operator.
    ```bash
    #!/bin/bash
    if test "A" = "A"
    then
      echo "Good job!"
    fi
    # Output: Good job!

    if test "A"="B"
    then
      echo "Good job!"
    fi
    # Output: Good job!
    ```
- **Brackets or [ ]**
  - The bash shell provides an alternative way of declaring the test command in an if-then statement.
    ```bash
    #!/bin/bash
    if [ CONDITION ]
    then
      COMMAND(S)
    fi
    ```
  - Be careful, you must have space after the first bracket, and a space before the last bracket or you’ll get an error message.
- There are three classes of conditions the test command can evaluate:
  - Numeric Comparison
      |Comparison|Description|
      |----------|-----------|
      |n1 -eq n2|Check if n1 is equal to n2|
      |n1 -ne n2|Check if n1 is not equal to n2|
      |n1 -ge n2|Check if n1 is greater than or equal to n2|
      |n1 -gt n2|Check if n1 is greater than to n2|
      |n1 -le n2|Check if n1 is less than or equal to n2|
      |n1 -lt n2|Check if n1 is less than to n2|
  - String Comparison
      |Comparison|Description|
      |----------|-----------|
      |str1 = str2|Check if str1 is the same as str2|
      |str1 != str2|Check if str1 is not the same as str2|
      |str1 < str2|Check if str1 is less than str2|
      |str1 > str2|Check if str1 is greater than str2|
      |-n str1|Check if str1 has a length greater than zero|
      |-z str1|Check if str1 has a length of zero|
  - File Comparison
      |Comparison|Description|
      |----------|-----------|
      |-d file|Check if file exists and is a directory|
      |-e file|Check if file exists|
      |-f file|Check if file exists and is a file|
      |-s file|Check if file exists and is not empty|
      |-r file|Check if file exists and is readable|
      |-w file|Check if file exists and is writable|
      |-x file|Check if file exists and is executable|
      |-O file|Check if file exists and is owned by the current user|
      |-G file|Check if file exists and the default group is the same as the current user|
      |file1 -nt file2|Check if file1 is newer than file2|
      |file1 -ot file2|Check if file1 is older than file2|
- Compound Condition Testing
  - **AND** or **&&** : Both conditions must be true for the then section to execute. `[ condition1 ] && [ condition2 ]` or `[ condition1 -a condition2 ]`
  - **OR** or **||** : At least one condition must be true that then section is executed. `[ condition1 ] || [ condition2 ]` or `[ condition1 -o condition2 ]`
  - **NOT** or **!**
    - `! expression`
    - `[ ! expression ]`
    - `if test ! expression`
    - `if [ ! condition ]`
- **Case**
  - The case statement is good alternative to Multilevel if-then-else-fi statement. It enable you to match several values against one variable. Its easier to read and write.
    ```bash
    #!/bin/bash
    case $VARIABLE in
      pattern1)
        # Commands to be executed if pattern1 matches
        COMMAND(s)
        ;;
      pattern2)
        # Commands to be executed if pattern2 matches
        COMMAND(s)
        ;;
      patternN)
        # Commands to be executed if patternN matches
        COMMAND(s)
        ;;
      *)
        # Default condition to be executed
        COMMAND(s)
    esac
    ```

## Part 09 (Repetitive Tasks)
