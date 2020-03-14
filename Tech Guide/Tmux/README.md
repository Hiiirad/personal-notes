# Tmux

Tmux is a [**terminal multiplexer**](https://en.wikipedia.org/wiki/Tmux) 
- **Concept:**
    - Within one terminal window, you can open multiple windows and split-views. (called**panes**)
    - Each pane will contain its own, independently running terminal instance
    - Allows you to have multiple terminal commands and applications running visually next to each other without any need to open multiple terminal emulator windows
    - Tmux keeps these windows and panes in a session. you can exit a session at any point. This is called **detaching**
    - Tmux will keep this session alive until you kill the tmux server (e.g. when you reboot)
    - This is incredibly useful because at any later point in time you can pick that session up exactly from where you left it by simply “attaching” to that session.

- **Use Cases:**
    - **Long-running processes:**
     When attached to a remote machine via ssh, long-running processes can be started and left running without needing to stay attached via ssh. If you’ve ever worked with remote servers over ssh you can guess where this will be useful: When you lose your ssh connection the tmux session will simply be detached but will keep running on the server in the background including all the processes that run within your session. To continue your session simply ssh to the server again and attach to the running session.
    - **Pair Programming:** Tmux can be used to allow two or more users to attach to the same session and share control providing an efficient pair programming setup.
    - **Local Development:** By enabling various layouts and communication between multiple processes, tmux can enable enhanced efficiency for local development.

## Installation
| OS | Command |
| --- | --- |
| MacOS  | brew install tmux  |
| FreeBSD  | sudo pkg install tmux  |
| Ubuntu, Mint, Debian, etc. | sudo apt-get install tmux |
| CentOS, Fedora, Redhat, etc. | sudo yum install tmux |
| Arch Linux | sudo pacman -S tmux |
| Windows | use a Linux machine! |

## Terminology

- **Session:**  the highest-level entity residing in the server instance. Server instances are forked to the background upon starting a fresh instance and reconnected to when reattaching sessions. Your interaction with tmux will have at least one session running. A named collection of one or more windows.
- **Window:** A single screen within tmux, similar to tabs in terminal applications. At any given time, a client will be attached to a single window.
- **Pane:** A portion of a window running a single process, e.g. Vim, zsh, man, etc. Panes can be oriented either vertically or horizontally and resized as needed.
