# Tmux

## Part 01 (Introduction)

Tmux abbreviated from [**Terminal Multiplexer**](https://en.wikipedia.org/wiki/Tmux) 
- **Concept:**
    - Within one terminal window, you can open multiple windows and split-views. (called **panes**)
    - Each pane will contain its own, independently running terminal instance.
    - It allows you to have multiple terminal commands and applications running visually next to each other without any need to open multiple terminal emulator windows.
    - Tmux keeps these windows and panes in a session. You can exit a session at any point. Which is called **detaching**
    - Tmux will keep this session alive until you kill the Tmux server (e.g., when you reboot)
    - This is incredibly useful because, at any later point in time, you can pick that session up exactly where you left it simply by “attaching” to that session.

- **Use Cases:**
    - **Long-running processes:**
     When attached to a remote machine via SSH, long-running processes can be started and left running without needing to stay attached via SSH. If you’ve ever worked with remote servers over SSH, you can guess where this will be useful. When you lose your SSH connection, Tmux session simply detached but will keep running on the server in the background, including all the processes that run within your session. To continue your session, you need SSH to the server again and attach to Tmux running session.
    - **Pair Programming:** Tmux can be used to allow two or more users to attach the same session and share control providing an efficient pair programming setup. Besides, you also share your screen by letting others attach to your Tmux session. (that is a cool and also weird way of using Tmux)
    - **Local Development:** By enabling various layouts and communication between multiple processes, Tmux can enable enhanced efficiency for local development.

## Part 02 (Installation)
| OS | Command |
| --- | --- |
| MacOS  | `brew install tmux`  |
| FreeBSD  | `sudo pkg install tmux`  |
| Debian-based | `sudo apt install tmux` |
| Fedora-based | `sudo yum install tmux` or `sudo dnf install tmux` |
| Arch Linux | `sudo pacman -S tmux` |
| Windows | use a Linux machine! |

There is a useful theme for showing windows name, date, etc. at the bottom of the terminal called Powerline, which you can easily install based on your distribution. (For example `sudo apt install powerline`)

## Part 03 (Terminology)

- **Session:**  The highest-level entity residing in the server instance. Server instances are forked to the background upon starting a fresh instance and reconnected to when reattaching sessions. Your interaction with Tmux will have at least one session running. A named collection of one or more windows.
- **Window:** A single screen within Tmux, similar to tabs in terminal applications. At any given time, a client will attach to a single window.
- **Pane:** A portion of a window running a single process, e.g., Vim, zsh, man, etc. Panes can be oriented either vertically or horizontally and resized as needed.

<img src="./images/tmux%20terminology.png"
     alt="tmux terminology"
     style="float: center;" />

## Part 04 (Commands)

- **Section 01 (First Rule of Tmux, what is prefix?)**
    - It is **`ctrl+b`** combination of keyboard strokes. </br>
    It's the magic word, used to start an interaction with Tmux server (a.k.a it is the way of calling Tmux server)
    - You can see all of Tmux powers and capabilities: `[prefix] + ?`

- **Section 02 (Session Management)**
    - Figure out which sessions are running: ```tmux ls```
    - Create a new session: ```tmux new -s <session_name>```
    - Connect (attach) to a existing Tmux session: ```tmux attach -t <session_name>```
    - Exit (detach) a currently running session: ```[prefix] + d```
    - Rename an existing session: ```tmux rename-session -t <previous_session_name> <new_session_name>```
    
- **Section 03 (Session Transmition)**
    - Switch the attached client to the previous session: `[prefix] + (`
    - Switch the attached client to the next session: `[prefix] + )`
    - Select a new session for the attached client interactively: `[prefix] + s`

- **Section 04 (Panes)**
    - Split pane vertically: `[prefix] + %`
    - Split pane horizontally: `[prefix] + "`
    - Switch between panes: `[prefix] + ARROWKEYS`
    - Switch the pane layout `[prefix] + o`
    - Zoom to pane and zoom back to normal size: `[prefix] + z`
    - Resize current pane height: `[prefix] + Ctrl + UP/DOWN`
    - Resize current pane width: `[prefix] + Ctrl + LEFT/RIGHT`
    - Show time in current pane: `[prefix] + t`
    - Activate/Deactivate synchronize panes: `[prefix] + :setw synchronize-panes on/off`
    - Scroll up or down: `[prefix] + PAGEUP/PAGEDOWN`
    - Search: `[prefix] + s`
    - Close current pane: `[prefix] + x` or `exit`

## Part 05 (References)

1. [Tmux cheatsheet](https://tmuxcheatsheet.com/)
