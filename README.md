# Cyber-Wall

VIRTUAL MACHINE & TOOL PREP <p>
Before proceeding, ensure you have the following:

A virtualized environment (VirtualBox, VMware), I used UTM as I own a Mac. On which you will install:
  Kali Linux ISO or pre-installed Kali Linux
  A local test environment where you will have your target system (I used Metasploitable)

How to set up UTM <p>
  Download UTM from the website: https://mac.getutm.app/ <p>
  Click download and follow the instructions
  
How to set up Kali Linux<p>
  Download Kali Linux from the website: https://www.kali.org/ <p>
  Click Download <p>
  Click Installer images and download the 64-bit version

  Launch UTM & click on Create new virtual machine <p>
  Select Virtualise <p>
  Select Linux (OS) <p>
  browse and select the ISO kali file, click continue <p>
  select hardware and storage preferences <p>
  Title your Kali Machine <p>
  Upon Starting the kali machine, select graphical install and answer installer questions as are appropriate to you <p>
  Please note that the installation process may take a few minutes to complete. Once finished, shut down Kali <p>
  Right click Kali and hit edit <p>
  On the left of the screen scroll down and delete the IDE Drive associated with the ISO drive <p>
  Start the Kali machine and begin!

How to set up Metasploitable 2
  Download Metasploitable 2 from website: https://docs.rapid7.com/metasploit/metasploitable-2/
  Select the link: https://information.rapid7.com/metasploitable-download.html
  Expand the zip file and convert the .vmdk file to 
    Open Terminal download the tool Qemu and navigate to the downloaded Metasploitable 2 folder
    Type: brew install qemu
          cd Downloads
          cd Metasploitable2-Linux (whatever the name of the file is)
          qemu-img convert -p -O qcow2 'Metasploitable.vmdk' Metasploitable.qcow2
          
  Launch UTM & click on Create new virtual machine
  Select Emulate
  Select Other
  Selct None for Boot Device
  Set memory to 512, storage 10 GB
  Click conitnue until you get to the Title page
  Title your Metasploitable machine
  Right click Metasploitable machine and hit edit
  On the left of the screen, select QEmU and deselect UEFI Boot
  Scroll down to the IDE Drive and delete it
  Then hit New (to create a new drive), then import, and navigate to and select the qcow2 file
  Start the Metasploitable machine, let it start up and begin!
  Note that the login credentials will be username: msfadmin password: msfadmin
  
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
>>Telnet: Use telnet to connect to remote servers.
>>Rsync: Use rsync for syncing files over SSH.
>>RDP: Use a tool like rdesktop or FreeRDP to establish an anonymous RDP session.
  
          
          

  
