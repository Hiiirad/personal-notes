# **Linux Essentials**

  - [Part 01 (Linux Main Directories)](#linux-main-directories)
  - [Part 02 (Log Directories)](#log-directories)
  - [Part 03 (Useful Commands)](#useful-directories)
  
## Part 01 (Linux Main Directories)

-   **/ (root)**

    Every file and project starts here. 
    Only root has a limit under the target. 
    This directory is different from the `/root` directory,
    which is the home directory of the root user. 

-	**/bin**

    Contains binary-to-run files. 
    All the commands used by the system are here.
    
    Such as: `ls`, `ping`, `grep`, `cp`, `ps`, etc.

-	**/sbin**  (System Binary Document)

    Just like `/bin`, `/sbin` also contains binary executable files.  
    However, commands under this directory are typically used by the system management. 
    
    For example: `iptables`, `reboot`, `fdisk`, `ifconfig`, `swapon` command.   

-	**/etc** (Configuration Document)

    Contains the profiles and configurations required by all programs. 
    It also contains a database of the programs.
    
    For example: `/etc/resolv.conf`, `/etc/logrotate.conf`

-	**/dev** Document

    These include terminals, USB, or any other devices connected to the system. 

    Example:`/dev/tty1`, `/dev/usbmon0`

-	**/proc** (Process message)

    Files under this directory basically includes information about the on-going process. For example: information about a specific pid that is included in the `/proc/pid` file.  

-	**/var**

    Contains variable data like system logging files, mail and printer spool directories, and transient and temporary files. Some portions of `/var` are not shareable between different systems. For instance, `/var/log`, `/var/lock`, and `/var/run`.

    Some of the most important /var subdirectories are listed below:
  
    - `/var/backups`: 
    
        Directory containing backups of various key system files such as `/etc/shadow`, `/etc/group`, `/etc/inetd.conf` and `dpkg.status`. They are normally renamed to something like `dpkg.status.0`, `group.bak`, `gshadow.bak`, `inetd.conf.bak`, `passwd.bak`, `shadow.bak`

    - `/var/cache`: 
    
        Is intended for cached data from applications. Such data is locally generated as a result of time-consuming I/O or calculation.

    - `/var/cache/man`: 
    
        A cache for man pages that are formatted on demand. 

    - `/var/cache/'PACKAGE-NAME'`: 
    
        Package specific cache data.

    - `/var/cache/www`: 

        WWW proxy or cache data.

    - `/var/db`: 
    
        Data bank store.

    - `/var/lib`: 
    
        Holds dynamic data libraries/files like rpm/dpkg databases.

    - `/var/lock`: 
    
        Many programs follow a convention to create a lock file in `/var/lock` to indicate that they are using a particular device or file. This directory holds those lock files (for some devices) and hopefully other programs will notice the lock file and won't attempt to use the device or file.

    - `/var/log`: 
    
        Log files from the system and various programs/services, especially login (`/var/log/wtmp`, which logs all logins and logouts into the system) and syslog (/var/log/messages, where all kernel and system program message are usually stored). Files in `/var/log` can often grow significantly, and may require cleaning at regular intervals. Something that is now normally managed via log rotation utilities such as 'logrotate'.

    - `/var/log/auth.log`: 
    
        Record of all logins and logouts by normal users and system processes.

    - `/var/log/btmp`: 
    
        Log of all attempted bad logins to the system. Accessed via `lastb` command.

    - `/var/log/dmesg`: 
        
        Kernel ring buffer. The content of this file is referred to by the dmesg command.

    - `/var/log/messages`: 
    
        System logs.

    - `/var/log/wtmp`: 
        
        Log of all users who have logged into and out of the system. The last command can be used to access a human readable form of this file. It also lists every connection and run-level change.

    - `/var/log/syslog`:
    
         The 'system' log file. The contents of this file is managed via the syslogd daemon which more often than not takes care of all log manipulation on most systems.

    - `/var/run`: 
    
        Contains the process identification files (PIDs) of system services and other information about the system that is valid until the system is next booted. For example, `/var/run/utmp` contains information about users currently logged in.

-   **/tmp** (Temporary Files)

    This directory contains mostly files that are required temporarily. Many programs use this to create lock files and for temporary storage of data. Do not remove files from this directory unless you know exactly what you are doing! 
    
    
-   **/usr** (Universal/User System Resources)

    - `/usr`:
    
        usually contains by far the largest share of data on a system. Hence, this is one of the most important directories in the system as it contains all the user binaries, their documentation, libraries, header files, etc.

    - `/usr/bin`: 
    
        contains the binary files of the programs. If you can't find a binary file in /bin, look at /usr/bin. Example: `awk`, `scp`, ... 
    
    - `/usr/sbin`: 
    
        contains a binary file for the system management binaries. If you can't find a system binary file in /sbin, check out /usr/sbin. Examples: `cron`, `sshd`, `useradd`, `userdel`. 

    - `/usr/lib`: 
    
        contains `/usr/bin` and `/usr/sbin` libraries.   
    
    - `/usr/local`: 
    
        contains the user program for the source installation. For example, when you install Apache from the source, it will be settle in `/usr/local/apache2`.   

-	**/home**

    All users use home to store their files and shit. 

-	**/boot**

    Contains files that are associated with the OS boot process. Also, the initrd, vmlinux, and grub files of the kernel are located under this directory.   

-	**/lib** 

    The /lib directory contains kernel modules and those shared library images (the C programming code library) needed to boot the system and run the commands in the root filesystem, ie. by binaries in /bin and /sbin. Libraries are readily identifiable through their filename extension of *.so. Windows equivalent to a shared library would be a DLL (dynamically linked library) file. 

    - `/lib/'machine-architecture'`: 
    
        Contains platform/architecture dependent libraries.

    - `/lib/iptables`: 
    
        iptables shared library files.

    - `/lib/kbd`: 
    
        Contains various keymaps.

    - `/lib/modules/'kernel-version'`: 
    
        The home of all the kernel modules. 

    - `/lib/security`: 
    
        PAM library files.

-	**/opt**  Additional applications
    
    This directory is reserved for all the software and add-on packages that are not part of the default installation. 

-	**/mnt**

    This is a generic mount point under which you mount your file-systems or devices. Mounting is the process by which you make a filesystem available to the system. After mounting your files will be accessible under the mount-point. 


 - **/srv**  (Services)

    contains site-specific data which is served by the system.

    This main purpose of specifying this is so that users may find the location of the data files for particular service, and so that services which require a single tree for readonly data, writable data and scripts (such as cgi scripts) can be reasonably placed. Data that is only of interest to a specific user should go in that users' home directory.

## Part 02 (Log Directories)

|  Directory |  Description |
|------------|--------------|
| `/var/log/cron` | main place to define user-level schedules  |
|  `/var/log/cups/`  | TODO |
| `/var/log/dmesg` | A letter was sent to the system that was checking itself at the time of opening. You can also use the dmesg command to look directly at the internal nuclear self-examination information |
| `/var/log/btmp` | Logs of the logged in users. This file is a binary file and cannot be viewed directly via vi or nano. but we can use lastb's help: <br>#lastb root tty1 Tue MAR 10 22:38 - 22:38 (00:00)|
| `/var/log/lastlog` | The last log-in time for the all users. This file is also a binary file You can't look directly with vi.|
| `/var/log/messages` | It is the main log file in Linux systems, which contains almost every action information around the system.  |
| `/var/log/secure` | TODO |
| `/var/log/wtmp` | TODO |
| `/var/tun/ulmp` | Information about the user that was previously logged in.|



## Part 03 (Useful Commands)


Check Who is the boss in the system:

```bash
awk -F: '$3==0{print 1}' /etc/passwd
```

Account information that can be logged in remotely

```bash
awk '/$1|$6/{print 1}' /etc/shadow
```

Check who is trying to login into your Linux systems

```bash
grep "root failed password" /var/log/auth.log | awk '{print 11}' | sort | uniq -c | sort -nr
```

The successful login date, user name and IP

```bash
grep "root failed password" /var/log/auth.log | awk '{print 11}' | sort | uniq -c | sort -nr
```

**lastList** , Logs the user information of the recent and previous login attempts.

```bash
last [-R] [-num] [ -n num] [-adiowx] [ -f file] [ -t YYYYMMDDHHMMSS] [name...] [tty...]
```
Example:
```bash
last -x : Shows history of system shutdown, user login and exit
last -i: Show the login of a specific ip
last -t 20210110220101: Show login information before 20210110220101
```

Check who has the sudo privilage

```bash
more /etc/sudoers | grep -v "^#\|^$" | grep "ALL=(ALL)"
```

Matching Defaults entries for root on OS

```bash
sudo -l
```

If you want to make SSH to a server without footprints on _who_, _last_ or similar commands:

```bash
ssh -T root@192.168.1.1 /bin/bash -i
ssh -o UserKnownHostsFile=/dev/null -T root@192.168.1.1 /bin/bash -if
```

If you need to hide your activity whitin your SSH Connection:

```bash
#Turn off history recording
#[space] represents space. And because of the space, the command itself will not be recorded.
[space] set +o history

# Turn on history recording
# Commands after executing commands will appear in history.
[Space] set -o history
```

View the process file information corresponding to pid

```bash
ps -ef | grep "process name" 
# gives you the PID of desired process 

ls -l /proc/$PID/ 
# You can view detailed process information

file /proc/$PID/exe 
# ($PID is the corresponding pid number)

cd /proc/$PID 
# can enter the process directory

which XXXX 
# Look which folder the program is in XXXX to represent the program name

lsof /usr/bin/* 
# View the list of running processes under a path

pidof /usr/bin/* 
# View the pid of the process running under a certain path
```

View scheduled tasks
```bash
crontab -l 
# View current user scheduled tasks

cat /etc/crontab 
# Check whether there is a scheduled task

ls -al /var/spool/cron/*
cat /var/spool/cron/* 
# Check if there are files for other scheduled tasks in the folder	

ls -al /etc/cron.d/*
for u in `cat /etc/passwd |cut -d ":" -f1`;do crontab -l -u $u; done 
# View all users' scheduled tasks

crontab -r 
# Delete scheduled tasks on the surface

crontab -e
# Edit the scheduled tasks
```

Interpretation of scheduled tasks:
```bash
#The first 5 stars represent minute-hour-day-month-week followed by the command
*   *   *   *   *   command 
# (indicates that it runs every 15 minutes)
*/15 *	 *	 *	 *	 Command

```



