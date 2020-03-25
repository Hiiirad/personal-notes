# Shell Scripting

> The minimum knowledge of your Linux and Unix systems should be LPIC 1 to understand shell scripting.

> This note is for SysAdmins or DevOps Engineers, so most of the commands of this course run on Redhat-based distributions such as Redhat, Centos, etc.

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
- Process ID of your shell:
  ```bash
  echo $$
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
- You can compile shell script to C using `shc`.
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
  ```
- Understand internal (built-in) or external command using `type COMMAND`.
  - If the command is builtin, you see an output like this: `COMMAND is a shell builtin.`
  - If the command isn't builtin, you can see 2 different output based on what you did:
    1. If you run `type COMMAND` before executing COMMAND, you see an output like this: `COMMAND is COMMAND_PATH`
    2. If you run `type COMMAND` after executing COMMAND, you see an output like this: `COMMAND is hashed (COMMAND_PATH)`
  - **Hash** means you just execute an external command, and you can see them by simply run `hash` command.
  - When an external command is executed, shell tells OS to make a fork from the shell and then run your external command in it and then exits. You can see Virtual Memory Statistics using this command: `vmstat -s`
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

## Part 02 (Shell Script Basics)

- If you want to run multiple commands in one line, you must separate commands with semicolon or `;`
  ```bash
  date ; whoami ; echo Hello World!
  ```
- You can on comment a line with `#` which has different names. (Sharp, Number, Pound, Hash or Hashtag)
- When creating a shell script file, you must specify the shell you are using only in the **first line** of file. It tells the OS what is the correct interpreter. The `#!` before shell path called **Shebang** and the format for this is:
  ```bash
  #!SHELL_PATH
  #!/bin/bash
  #!/bin/sh
  ```
- It's recommended that you put `.sh` extension at the end of your script file.
- Variables are **case-sensitive**.
- It's recommended that you set your variables in **UPPERCASE** letters.
- Values are assigned to user variables using an equal sign. **No
spaces** can appear between the variable, the equal sign, and the
value.
- The shell script automatically determines the data type used for
the variable value.

## Part 03 (Variables)
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
  - **Global Variable**: Global variables, which are called environment variables, are visible from the shell session, and any child processes that the shell spawns.
  - **Local Variable**: Local variables are only available in the shell that creates them. This makes global environment variables useful in applications that spawn child processes that require information from the parent process.
- You can see all variables (Global + Local) with `set` command.
- You can see Global variables with `env` or `printenv` command.
- The method used to create a global environment variable is to create a local environment variable, then export it to the global environment. This is done by using the `export` command:
  ```bash
  VAR1=test
  export VAR1
  ```
- Exported variables and their values are copied into a subshell's environment, where they may be accessed and changed. However, such changes have no effect on the variables in the parent shell.
- You can remove variables by using `unset` command.
  ```bash
  VAR1=testing
  unset VAR1
  echo VAR1   # No output
  ```
  - When referencing the environment variable in the unset command, remember **not to use the dollar sign or `$` before variable name**.
  - If you’re in a child process and unset a global environment variable, it only applies to the child process. The global environment variable is still available in the parent process.
