# Cyber-Wall

VIRTUAL MACHINE & TOOL PREP <p>
Before proceeding, ensure you have the following:

A virtualized environment (VirtualBox, VMware), I used UTM as I own a Mac. On which you will install:
  Kali Linux ISO or pre-installed Kali Linux
  A local test environment where you will have your target system (I used Metasploitable)

**How to set up UTM** <p>
  Download UTM from the website: https://mac.getutm.app/ <p>
  Click download and follow the instructions
  
**How to set up Kali Linux** <p>
  Download Kali Linux from the website: https://www.kali.org/ <p>
  Click Download <p>
  Click Installer images and download the 64-bit version <p>

  Launch UTM & click on Create new virtual machine <p>
  Select Virtualise <p>
  Select Linux (OS) <p>
  Browse and select the ISO kali file, click continue <p>
  Select hardware and storage preferences <p>
  Title your Kali Machine <p>
  Upon Starting the kali machine, select graphical install and answer installer questions as are appropriate to you <p>
  Please note that the installation process may take a few minutes to complete. Once finished, shut down Kali <p>
  Right click Kali and hit edit <p>
  On the left of the screen scroll down and delete the IDE Drive associated with the ISO drive <p>
  Start the Kali machine and begin! <p>

**How to set up Metasploitable 2** <p>
  Download Metasploitable 2 from website: https://docs.rapid7.com/metasploit/metasploitable-2/ <p>
  Select the link: https://information.rapid7.com/metasploitable-download.html <p>
  Expand the zip file and convert the .vmdk file to <p>
    Open Terminal download the tool Qemu and navigate to the downloaded Metasploitable 2 folder <p>
    Type: brew install qemu <p>
          cd Downloads <p>
          cd Metasploitable2-Linux (whatever the name of the file is) <p>
          qemu-img convert -p -O qcow2 'Metasploitable.vmdk' Metasploitable.qcow2 <p>
          
  Launch UTM & click on Create new virtual machine <p>
  Select Emulate <p>
  Select Other <p>
  Selct None for Boot Device <p>
  Set memory to 512, storage 10 GB <p>
  Click conitnue until you get to the Title page <p>
  Title your Metasploitable machine <p>
  Right click Metasploitable machine and hit edit <p>
  On the left of the screen, select QEmU and deselect UEFI Boot <p>
  Scroll down to the IDE Drive and delete it <p>
  Then hit New (to create a new drive), then import, and navigate to and select the qcow2 file <p>
  Start the Metasploitable machine, let it start up and begin! <p>
  Note that the login credentials will be username: msfadmin password: msfadmin <p>

**How to set up Windows 11**



MODULE 1
**FTP: Use ftp command in Kali to connect anonymously and list directories**
  Use command 'ifconfig' to identify the ip addresses on your network. You would find the ip addresses of your Kali and Metasploitable machine
  These are mine:
    Kali: 192.168.64.2
    Metaploitable (Meta): 192.168.64.4
    Nmap scan shows the results of a TCP scan on port 21. 
      192.168.64.4: is the target IP address.
      -v: verbose gives details
      -sT: scan TCP
      -SV: identify services on the port
      -p 21: scans port 21
      -O: shows the target's OS
  <img width="1171" alt="FTP1" src="https://github.com/user-attachments/assets/c7efc627-5651-4069-a5d9-98e16bb4703c" />
  <img width="1028" alt="FTP2" src="https://github.com/user-attachments/assets/321b52f1-3ff7-4371-8ddc-c0c23f6c31bb" />
  
  I elevated my permission to root using command 'sudo su'
  Using the command 'ftp 192.168.64.4', you can access the Meta machine
  <img width="559" alt="FTP3" src="https://github.com/user-attachments/assets/1d1b0bd7-c0eb-4599-84af-4bd32aa9fcaf" />

**SMB: Use smbclient to access shared resources**
 We want to list the accessible ports with nmap
 Use command: nmap -sV 192.168.164.4 -vv
 Then list the available shared files on the target machine
 Use command: enum4linux -L -S 192.168.164.4
 <img width="940" alt="SMB1" src="https://github.com/user-attachments/assets/21f0064a-19ef-4938-b57b-c57bee96f19b" />
 <img width="1208" alt="SMB2" src="https://github.com/user-attachments/assets/b5febfb7-8794-4208-ae2d-8fd1fe0b0878" />

 Use Metasploit with command: msfconsole
 Using symlink_traversal to see the directory access
 <img width="1253" alt="SMB3" src="https://github.com/user-attachments/assets/0483e664-a5b4-4b64-80b8-853930f7b8e5" />
 This then shows the tmp file being available:
 <img width="1130" alt="SMB4" src="https://github.com/user-attachments/assets/70c90d71-948b-422b-bf8e-bcac834ca8af" /> 
 Set the rhosts to the target machine, smbshare to the shared tmp file, then use the command 'run' to exploit the machine
 <img width="1279" alt="SMB5" src="https://github.com/user-attachments/assets/2af7681d-169e-464b-beb5-d0c4f918b99b" />
 Then use the following command to login and access the files:
 <img width="1005" alt="SMB_final" src="https://github.com/user-attachments/assets/cd98673c-730d-4a5a-ada5-54300771ae02" />
 
**Telnet: Use telnet to connect to remote servers**
  Access Metasploit
  To utilise Telnet use the command: auxiliary/scanner/telnet/
  <img width="1321" alt="Telnet1" src="https://github.com/user-attachments/assets/b41830fe-3d03-4df8-b20e-5470ef2b07c6" />
  Use the command 'show options' to list the various settings to be configured. Set the rhosts to your target machine's IP address and run the exploit to gain access.
  <img width="1319" alt="Telnet_final" src="https://github.com/user-attachments/assets/6f74d7bd-34ce-456e-8b3b-cded818342cd" />
<p>  
**Rsync: Use rsync for syncing files over SSH**
<p> <p>
**RDP: Use a tool like rdesktop or FreeRDP to establish an anonymous RDP session** <p>
  To access the Windows machine using rdesktop: use the command: rdesktop -u [username] -p [password] [host:port] <p>
  If unable to gain remote access, ensure you have access by: <p>
    - ping target machine <p>
    - ensure RDP is turned on <p>
    - ensure the firewall (like Windows Defender is off) <p>
   <i> (for RDP a new Windows machine was created due to previous incompatible OS for the tool) 
<img width="727" alt="RDP1" src="https://github.com/user-attachments/assets/d01c2f6f-330f-4a75-ac81-5ced22f1e5ba" />


MODULE 2: PORT SCANNING WITH NMAP

**Perform a basic Nmap scan against a target machine to identify open ports and services**
Using command: nmap [IP address] it scans your network and shows the various connected devices
<img width="669" alt="nmap1" src="https://github.com/user-attachments/assets/a55d12e6-b6e2-4efa-8a78-f970c84ef133" />

**Try scanning with different Nmap flags, including -sS (TCP SYN scan), -sU (UDP scan), and -O (OS detection)**
nmap with TCP SYN scan - used to determine if any ports are open on the target machine
<img width="773" alt="nmap2" src="https://github.com/user-attachments/assets/7b5d1671-4f4d-4b0a-b4b6-dab0ba8d2899" />

nmap with UDP scan - used to identify available UDP ports on a target machine. Below shows nothing
<img width="838" alt="nmap3" src="https://github.com/user-attachments/assets/2e488854-b2d7-4ae4-bc4a-ff56c578e9f0" />

nmap with OS detection - identifies the operating system on the target machine
<img width="726" alt="nmap4" src="https://github.com/user-attachments/assets/6226f4c2-91cd-4edf-9db4-365a5918a28d" />

**Document the output, and analyze which ports are open and what services they correspond to**
The nmap scan shows 23 open ports which highlights various means by which to access the machine, including 21 (FTP - for file tranfers), 22 (SSH - allows secure access to a machine allowing access to files, commands, etc) and port 80 (HTTP - to load webpages, send server requests, etc.)
<img width="729" alt="nmap5" src="https://github.com/user-attachments/assets/63e9a36e-e128-4f74-a7d4-3f745d50d718" />

MODULE 3: LEARN HOW TO CONNECT TO A MONGODB SERVER
Jade!instructions: https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/#std-label-install-mdb-community-ubuntu

**Set up a MongoDB server or find a vulnerable MongoDB server to connect to**
curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg \
   --dearmo
**Use MongoDB tools like mongo to establish a connection to the server**

**Explore the database by listing available databases and checking for any vulnerabilities**

  
          
          

  
