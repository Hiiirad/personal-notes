# **Linux Essentials**

  - [Part 01 (Linux Main Directoriess)](#linux-main-directories)
  - [Part 02 (Log Directories)](#log-directories)
  
## Part 01 (Linux Main Directories)


- **/ (root)**

    Every file and project starts here. 
    Only root has  a limit under the target. 
    This directory is different from the /root directory,
    which is the main directory of the root user. 

-	**/bin**

    Contains binary-to-run files. 
    All the commands used by the system are here.
    
    Such as: ls, ping, grep, cp, ps, etc...

-	**/sbin**  (System Binary Document)

    Just like /bin, /sbin also contains binary runable files.  
    However, commands under this directoey are typically used by the system management. 
    
    For example: iptables, reboot, fdisk, ifconfig, swapon command.   

-	**/etc** (Configuration Document)

    Contains the profiles and configurations required by all programs. 
    It also contains a database of the programs. 
    
    For example:`/etc/resolv.conf`, `/etc/logrotate.conf`


-	**/dev** Document 

    These include terminals, USB, or any other devices connected to the system. 
    
    Example:`/dev/tty1`, `/dev/usbmon0`


-	**/proc** (Process message)

    Files under this directory basically includes information about the on-going process. For example: information about a specific pid that is included in the/proc/pid file.  


-	**/var**

    TODO


-	**/tmp** (temporary stuff)

    When the system reboots, all the files here will be gone.


-	**/usr**   

    Directory which contains binary files, files, documents, and user defined programs. 

    `/usr/bin`: contains the binary files of the programs. If you can't find a binary file in /bin, look at /usr/bin. Example: awk, scp ... 
    
    `/usr/sbin`: contains a binary file for the system management system. If you can't find a system binary file in /sbin, check out /usr/sbin.  Examples:atd,cron,sshd,useradd,userdel. 

    `/usr/lib`: contains /usr/bin and /usr/sbin.   
    
    `/usr/local`: contains the user program for the source installation. For example, when you install Apache from thesource, it will be in /usr/local/apache2.   


-	**/home**

    All users use home to store their files. 


-	**/boot**

    Contains files that are associated with the OS boot process. also the initrd, vmlinux,and grub files of the kernel are located under this directory.   

-	**/lib** 

    TODO

-	**/opt**  Additional application s
    TODO 

-	**/mnt**

    TODO 

-	**/media**  (Transferable Media)

    CD-ROM/ medias and stuff.

 - **/srv**  (Services)
    TODO


## Part 02 (Log Directories)

|  Directory |  Description |
|---|---|
| `/var/log/cron` | main place to define user-level schedules  |
|  `/var/log/cups/`  | TODO |
| `/var/log/dmesg` | A letter was sent to the system that was checking itself at the time of opening. You can also use the dmesg command to look directly at the internal nuclear self-examination information |
| `/var/log/btmp` | Logs of the logged in users. This file is a binary file and cannot be viewed directly via vi or nano. but we can use lastb's help: <br>#lastb root tty1 Tue MAR 10 22:38 - 22:38 (00:00)|
| `/var/log/lasllog` | The last log-in time for the all users. This file is also a binary file You can't look directly with vi.|
| `/var/log/messages` | It is the main log file in Linux systems, which contains almost every action information around the system.  |
| `/var/log/secure` | TODO |
| `/var/log/wtmp` | TODO |
| `/var/tun/ulmp` | Information about the user that was previously logged in.|

