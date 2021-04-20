The goal of the Linux Standard Base (LSB) is to develop and promote a set of standards and guidelines to promote compatibility between Linux operating systems

1 - What is a Linux standard base ? --> It is a join project by Linux foundation and various other linux distributions based on POSIX (Portable operating system interface)

2 - What are the areas of standardization by the Linux foundations? --> Standard Libraries , File system layout , common utitlities , run levels , Boot facilities.
backward compatibilites.

SysVinit is an initialization process for Linux systems that begins with a single process, init, which subsequently spawns all other processes on the system. This is a old
process and most of the recent versions have moved to systemd.

if you want to check the various run levels provided by sys-init , you have to go to /etc/iniitab ... this file is not being used in the modern version of systemd.

Diff run levels
0 - hault (never use this)
1 - single user mode
2 - multiuser mode with NFS
3 - full multiuser mode 
4 - unused
5 - X11
6 - reboot

What is the first script that gets called out as part of boot up ?

The init.d directory contains initialization scripts for all the services on our system. 
These scripts can be called to stop and start a service, and can be modified to enable or disable a service for a specific runlevel. 
Based on the run levels configured it goes to /etc/rc.d and excuted the files in the directory based on the configured run levels

[centos@sbphcdg175-usw2 rc.d]$ ls -ltr
total 4
-rw-r--r--. 1 root root 473 Apr 11  2018 rc.local
drwxr-xr-x. 2 root root  84 Jul  2  2020 init.d
drwxr-xr-x. 2 root root  62 Jul  2  2020 rc5.d
drwxr-xr-x. 2 root root  62 Jul  2  2020 rc4.d
drwxr-xr-x. 2 root root  62 Jul  2  2020 rc3.d
drwxr-xr-x. 2 root root  62 Jul  2  2020 rc2.d
drwxr-xr-x. 2 root root  62 Jul  2  2020 rc1.d
drwxr-xr-x. 2 root root  62 Jul  2  2020 rc0.d
drwxr-xr-x. 2 root root  62 Jul  2  2020 rc6.d

For example if I navigate to one of the run level directories I will be able to find out what are the system services that will start or stop 
based on the K or the S links

[centos@sbphcdg175-usw2 rc.d]$ cd rc3.d/
[centos@sbphcdg175-usw2 rc3.d]$ ls -ltr
total 0
lrwxrwxrwx. 1 root root 20 May 16  2018 K50netconsole -> ../init.d/netconsole
lrwxrwxrwx. 1 root root 17 May 16  2018 S10network -> ../init.d/network
lrwxrwxrwx. 1 root root 16 Jul  2  2020 S90splunk -> ../init.d/splunk

Here the K links will be killed and S links will be started.

How to modify the run levels ?

You can modify the init file itself under the various run levels of the particular service. 
But in order for it to get affected you need delete and create the service back.


How to check the current run level ?

[centos@sbphcdg175-usw2 rc4.d]$ runlevel
N 3
The above output indicates the current runlevel is 3 and there was no previous run levels.

The init.d directory contains initialization scripts for all the services on our system. 
These scripts can be called to stop and start a service, and can be modified to enable or disable a service for a specific runlevel.

Systemd has become the standard initialization process for most Linux Distributions. 
Unlike SysVinit which started service one after the other, Systemd implements a parallel model. 

When the system is powered on first the BIOS (Basic Input and Output system) gets called for or UEFI (Unified extensible firmware interface - 
only in modern system ).

BIOS or UEFI otherwise called as firmware. Firware is the code tied up to any hardware.

It goes on checks if all the input and output devices are all setup correctly.

If all the checks pass then it looks out for MBR (Master boot record ) in one of the connected devices and loads it up and passed it to bootloader.

Different type of bootloaders are there , in our case it will be grub (grub version2 )

It will load the kernel and the initramfs images. the kernel then loads the root file system and runs the main initalization process.









