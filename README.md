# Cyber-Wall

MACHINE & TOOL PREP
Before proceeding, ensure you have the following:

A virtualized environment (VirtualBox, VMware), I used UTM as I own a Mac. On which you will install:
  Kali Linux ISO or pre-installed Kali Linux
  A local test environment where you will have your target system (I used Metasploitable)

How to set up UTM
  Download UTM from the website: https://mac.getutm.app/
  Click download and follow the instructions
  
How to set up Kali Linux
  Download Kali Linux from the website: https://www.kali.org/
  Click Download
  Click Installer images and download the 64-bit version

  Launch UTM & click on Create new virtual maachine
  Select Virtualise
  Select Linux (OS)
  browse and select the ISO kali file, click continue
  select hardware and storage preferences
  Title your Kali Machine
  Upon Starting the kali machine, select graphical install and answer installer questions as are appropriate to you
  Please note that the installation process may take a few minutes to complete. Once finished, shut down Kali
  Right click Kali and hit edit
  On the left of the screen scroll down and delete the IDE Drive associated with the ISO drive
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
  FTP: Use ftp command in Kali to connect anonymously and list directories.
  <img width="1171" alt="FTP1" src="https://github.com/user-attachments/assets/ff642266-5032-4ac7-b1d8-f1d0b3473e69" />

  SMB: Use smbclient to access shared resources.
  Telnet: Use telnet to connect to remote servers.
  Rsync: Use rsync for syncing files over SSH.
  RDP: Use a tool like rdesktop or FreeRDP to establish an anonymous RDP session.
  
          
          

  
