# Shell Scripting

> The minimum knowledge of your Linux and Unix systems should be LPIC 1 to understand shell scripting.

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

## Part 02 (Shell Script Basics)

- If you want to run multiple commands in one line, you must separate commands with semicolon or `;`
- You can on comment a line with `#` which has different names. (Sharp, Number, Pound, Hash or Hashtag)
- When creating a shell script file, you must specify the shell you are using only in the **first line** of file. The `#!` before shell path called **Shebang** and the format for this is:
  ```bash
  #!SHELL_PATH
  #!/bin/bash
  #!/bin/sh
  ```
- Variables are **case-sensitive**.
- It's recommended that you set your variables in **UPPERCASE** letters.
- Values are assigned to user variables using an equal sign. **No
spaces** can appear between the variable, the equal sign, and the
value.
- The shell script automatically determines the data type used for
the variable value.
- Using variable value preceded by a dollar sign or `$`
  ```bash
  VAR1=27
  VAR2=$VAR1
  ```
- The backtick (`\``) allows you to assign the output of a shell command
to a variable. You must surround the entire command line command with
backtick characters:
  ```bash
  TODAY=`date`
  echo $TODAY
  # or
  echo `date`
  ```
