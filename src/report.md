#Report

## Part 1. Installation of the OS
Install Ubuntu 20.04 Server LTS without GUI

`$ cat /etc/issue`

![linux](/src/src/images/linux1_1.png)


## Part 2. Creating a user
Create a user other than the one created during installation. The user must be added to adm group.

- 2.1 Create a user other than the one created during installation

`$ sudo adduser alex`

![linux](/src/src/images/linux2_1.png)

- 2.2 The new user must be in the output of the command:

`$ cat /etc/passwd`

![linux](/src/src/images/linux2_2.png)

- 2.3 The user must be added to adm group.

`$ cat /etc/passwd | grep alex`

CHECK!!!

## Part 3. Setting up the OS network

- 3.1 Set the machine name as user-1

`$ sudo hostnamectl set-hostname user-1`

`$ sudo hostname user-1`

`$ hostnamectl`

![linux](/src/src/images/linux3_1.png)

- 3.2 Output the names of the network interfaces using a console command

`$ sudo timedatectl set-timezone Europe/Moscow`

`$ timedatectl`

![linux](/src/src/images/linux3_2.png)

- 3.3 Output the names of the network interfaces using a console command.

`$ ip -br link show`

![linux](/src/src/images/linux3_3.png)

    In the report give an explanation for the presence of the lo interface.

lo (loopback device) – виртуальный интерфейс, присутствующий по умолчанию в любом Linux. Он используется для отладки сетевых программ и запуска серверных приложений на локальной машине. С этим интерфейсом всегда связан адрес 127.0.0.1. У него есть dns-имя – localhost. Посмотреть привязку можно в файле /etc/hosts.

- 3.4 Use the console command to get the ip address of the device you are working on from the DHCP server.

`$ ip address show`

![linux](/src/src/images/linux3_4.png)

    Decode DHCP in the report.

DHCP (Dynamic Host Configuration Protocol) is a network management protocol used to dynamically assign an IP address to any device, or node, on a network so it can communicate using IP.

- 3.5 Define and display the external ip address of the gateway (ip) and the internal IP address of the gateway, aka default ip address (gw).

`$ curl curlmyip.ru`

![linux](/src/src/images/linux3_5_0.png)

`$ ip route`

![linux](/src/src/images/linux3_5_1.png)

- 3.6 Set static (manually set, not received from DHCP server) ip, gw, dns settings (use public DNS servers, e.g. 1.1.1.1 or 8.8.8.8).

Change to static.

`$ sudo vim /etc/netplan/00-installer-config.yaml`

![linux](/src/src/images/linux3_6.png)

`$ sudo netplan apply` - apply new configuration

![linux](/src/src/images/linux3_6_1.png)

![linux](/src/src/images/linux3_6_2.png)

- 3.7 Reboot the virtual machine. Make sure that the static network settings (ip, gw, dns) correspond to those set in the previous point.

`$ reboot`

`$ cat /etc/netplan/00-installer-config.yaml`

![linux](/src/src/images/linux3_7_0.png)

    Successfully ping 1.1.1.1 and ya.ru remote hosts and add a screenshot of the output command to the report. There should be "0% packet loss" phrase in command output.

`$ ping 1.1.1.1`

![linux](/src/src/images/linux3_7_1.png)

`$ ping ya.ru`

![linux](/src/src/images/linux3_7_2.png)


## Part 4. OS Update

 Update the system packages to the latest version

 `$ sudo apt-get dist-upgrade`

 ![linux](/src/src/images/linux4.png)
 

## Part 5. Using the sudo command

    Allow user created in Part 2 to execute sudo command.

`$ sudo hostnamectl set-hostname alex`

`$ hostnamectl`

 ![linux](/src/src/images/linux5.png)

 sudo is a program for Unix-like computer operating systems that enables users to run programs with the security privileges of another user, by default the superuser.


 ## Part 6. Installing and configuring the time service

    Set up the automatic time synchronisation service.

`$ sudo timedatectl`

`$ timedatectl show`

![linux](images/linux6.png)


## Part 7. Installing and using text editors

## - VIM

`$ sudo apt install vim` - Installing VIM

`$ vim test_vim.txt` - Creating a txt file by VIM

![linux](images/linux7_1.png)

- Esc 
- :wq - Exit with saving
- Enter

![linux](images/linux7_2.png)

- Esc
- :q! - Exit without saving
- Enter

![linux](images/linux7_3.png)

- /'!' - Search for

![linux](images/linux7_4.png)

- s/'Hello again!' - Replacement

## - NANO

`$ sudo apt install nano` - Installing NANO

`$ nano test_nano.txt` - Creating a txt file by NANO

- Ctrl + X - Exit
- Y - Save

![linux](images/linux7_5.png)

- Ctrl + X - Exit
- N - Don't save

![linux](images/linux7_6.png)

For searching press Ctrl+W. 
Word that should be replaced need to write and press Ctrl+R - Enter.
Write replacing word and press Enter.
Press Y for replacement.

![linux](images/linux7_7.png)

## - JOE

`$ sudo apt install joe` - Installing JOE

`$ joe test_joe.txt` - Creating txt file by JOE

![linux](images/linux7_8.png)

- Ctrl+K, Q, Y - Exit with saving

![linux](images/linux7_9.png)

- Ctrl+K, Q, N - Exit without saving

![linux](images/linux7_10.png)

![linux](images/linux7_11.png)

- Ctrl+K, F - Search

![linux](images/linux7_12.png)

- Ctrl+K, F, R, Y - Replacement

![linux](images/linux7_13.png)


## Part 8. Installing and basic setup of the SSHD service

- 8.1 Install the SSHd service

`$ sudo apt-get install ssh`

![linux](images/linux8_1.png)

`$ sudo apt install openssh-server`

![linux](images/linux8_2.png)

- 8.2  Add an auto-start of the service whenever the system boots

`$ sudo systemctl enable ssh`

![linux](images/linux8_2_1.png)

`$ systemctl status ssh`

![linux](images/linux8_2_2.png)

- 8.3 Reset the SSHd service to port 2022.

`$ sudo nano /etc/ssh/sshd_config`

- 8.4 Show the presence of the sshd process using the ps command. To do this, you need to match the keys to the command.

`$ ps -auxf | grep ssh`

![linux](images/linux8_4_1.png)

    Explain in the report the meaning of the command and each key in it.

    ps - report a snapshot of the current processes.

           -a   Select all processes except both session leaders and processes not associated with a terminal.

           -u   Select by effective user ID (EUID) or name.  This selects the processes whose effective user name or ID is in userlist.

           -x   View all processes owned by you

           -f   Do full-format listing.

- 8.5 Reboot the system.

`$ sudo reboot now`

    The output of the netstat -tan command should contain
    tcp 0 0.0.0.0:2022 0.0.0.0:* LISTEN

`$ netstat -tan`

![linux](images/linux8_5_1.png)

    Explain the meaning of the -tan keys, the value of each output column, the value 0.0.0.0 in the report.

>-t	Showing the current TCP chimney offload state in place of the typically displayed TCP state.

>-a	Displays active TCP connections, TCP connections with the listening state, as well as UDP ports that are being listened to.

>-n	Preventing netstat from attempting to determine host names for foreign IP addresses.


>Proto: Connection protocol

>Recv-Q: Network Receive Queue

>Send-Q: Network Send Queue

>Local Address: IP address of the local computer and the port number used

>Foreign Address: IP address and port number of the remote computer connected to this socket

>State: TCP connection status

> 0.0.0.0 defines an IP block containing all possible IP addresses. It is commonly used in routing to depict the default route as a destination subnet. It matches all addresses in the IPv4 address space and is present on most hosts, directed towards a local router.


# Part 9. Installing and using the top, htop utilities

- 9.1 Install and run the top and htop utilities

`$ sudo apt install htop`

![linux](images/linux9_1_1.png)

`$ top`

![linux](images/linux9_1_2.png)

    From the output of the top command determine and write in the report:
>uptime - 4 min;
>number of authorised users - 1;
>total system load - 0.01, 0.04, 0.01;
>total number of processes - 98;
>cpu load - 0.0%;
>memory load - 145.1;
>pid of the process with the highest memory usage - 1;
>pid of the process taking the most CPU time - none;

    Add a screenshot of the htop command output to the report:
>sorted by PID, PERCENT_CPU, PERCENT_MEM, TIME

`$ htop --sort-key PID`

![linux](images/linux9_1_4.png)

`$ htop --sort-key PERCENT_CPU`

![linux](images/linux9_1_7.png)

`$ htop --sort-key PERCENT_MEM`

![linux](images/linux9_1_6.png)

`$ htop --sort-key TIME`

![linux](images/linux9_1_3.png)

>filtered for sshd process

`$ htop`

- F4

![linux](images/linux9_1_5.png)

>with the syslog process found by searching

- F3

![linux](images/linux9_1_8.png)

>with hostname, clock and uptime output added

- F2 CHECK!!!!

![linux](images/linux9_1_9.png)


## Part 10. Using the fdisk utility

`$ sudo fdisk -l`

![linux](images/linux10_1.png)

    In the report write the name of 
>the hard disk - VBOX HARDDISK
>capacity - 10GiB
>number of sectors - 20971520
>the swap size - CHECK!!!

`$ free -h`

![linux](images/linux????.png)


## Part 11. Using the df utility

Run the df command.

`$ df`

![linux](images/linux11_1.png)

    In the report write for the root partition (/):
>partition size - 8408452
>space used - 4201588
>space free - 3758148
>percentage used - 53%
>Determine and write the measurement unit in the report - Kilobyte

Run the df -Th command.

`$ df -Th`

![linux](images/linux11_2.png)

    In the report write for the root partition (/):
>partition size - 8.1G
>space used - 4.1G
>space free - 3.6G
>percentage used - 53%
>Determine and write the file system type for the partition in the report - ext4


## Part 12. Using the du utility

Run the du command.

Output the size of the /home, /var, /var/log folders (in bytes, in human readable format)

`$ sudo du -B1 -d0 /home /var/log /var`

![linux](images/linux12_1.png)

![linux](images/linux12_2.png)

Output the size of all contents in /var/log (not the total, but each nested element using *)

`$ sudo du -h /var/log/*`

![linux](images/linux12_3.png)


## Part 13. Installing and using the ncdu utility

Install the ncdu utility.

`$ sudo apt install ncdu`

Output the size of the /home, /var, /var/log folders.

`$ sudo ncdu /`

![linux](images/linux13_1.png)

![linux](images/linux13_2.png)


## Part 14. Working with system logs

Open for viewing:
1. /var/log/dmesg

`$ sudo vim /var/log/dmesg`

![linux](images/linux14_1.png)

2. /var/log/syslog

`$ sudo vim /var/log/syslog`

![linux](images/linux14_2.png)

3. /var/log/auth.log

`$ sudo vim /var/log/auth.log`

![linux](images/linux14_3.png)

    Write the last successful login time - Jun 26 04:59:22;
>user name - fletamar;
>login method in the report - pam_unix;

    `$ sudo cat /var/log/auth.log | grep login`

![linux](images/linux14_4.png)

    Restart SSHd service.

    `$ sudo systemctl restart sshd`

    `$ cat /var/log/syslog | grep ssh`

![linux](images/linux14_5.png)


## Part 15. Using the CRON job scheduler

Using the job scheduler, run the uptime command in every 2 minutes.

`$ sudo crontab -e`

![linux](images/linux15_1.png)

>Find lines in the system logs (at least two within a given time range) about the execution.

`$ grep -i cron /var/log/syslog`

![linux](images/linux15_2.png)

>Display a list of current jobs for CRON.

`$ sudo crontab -l`

![linux](images/linux15_3.png)

Remove all tasks from the job scheduler.

`$ sudo crontab -r`

`$ sudo crontab -l`

![linux](images/linux15_4.png)
