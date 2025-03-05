# Cyber-Wall

VIRTUAL MACHINE & TOOL SETUP
Before starting, make sure you have:
<br> <br>
A virtualized environment (like VirtualBox, UTM or VMware). I used UTM because I’m on a Mac.<br>
Kali Linux (either the ISO or pre-installed version).<br>
A local test environment with your target system (e.g., Metasploitable).<br>
<br><br>
How to Set Up UTM (for Mac)<br>
  Go to the UTM website: https://mac.getutm.app/<br>
  Download UTM and follow the installation instructions.<br>
<br><br>
How to Set Up Kali Linux in UTM<br>
  1. Download Kali Linux from https://www.kali.org/:<br>
    Click "Download".<br>
    Under "Installer images", select and download the 64-bit version.<br>
<br>
  2. Open UTM and click Create new virtual machine.<br>
    Choose Virtualize.<br>
    Select Linux (OS).<br>
    Browse and select the Kali Linux ISO file, then click Continue.<br>
 <br>
  3. Choose your hardware and storage preferences.<br>
    Title your Kali machine.<br>
 <br>
  4. Start the Kali machine:<br>
    Select Graphical install and follow the installation prompts.<br>
    The installation process may take a few minutes.<br>
    After it finishes, shut down Kali.<br>
 <br>
  5. Right-click the Kali machine and click Edit.<br>
    Scroll to find the IDE Drive associated with the ISO and delete it.<br>
  <br>
  6. Start the Kali machine, and you’re ready to go!<br>
<br>
How to Set Up Metasploitable 2<br>
  1. Download Metasploitable 2 from this link and expand the zip file.<br>
<br>
  2. Convert the .vmdk file to .qcow2 format:<br>
    Open Terminal and install Qemu:<br>
      brew install qemu<br>
    Navigate to the folder where you downloaded Metasploitable 2:<br>
      cd Downloads<br>
      cd Metasploitable2-Linux<br>
    Convert the file using this command:<br>
      qemu-img convert -p -O qcow2 'Metasploitable.vmdk' Metasploitable.qcow2<br>
 <br>   
  3. Open UTM and click Create new virtual machine.<br>
    Select Emulate and choose Other.<br>
    Choose None for Boot Device.<br>
    Set memory to 512 MB and storage to 10 GB.<br>
    Continue until you reach the Title page, and give your Metasploitable machine a name.<br>
<br>
  4. Right-click the Metasploitable machine and click Edit.<br>
    Select QEMU on the left.<br>
    Deselect UEFI Boot.<br>
    Scroll to the IDE Drive and delete it.<br>
<br>
  5. Click New to create a new drive.<br>
    Choose Import and navigate to the qcow2 file you created earlier.<br>
    Select it and finish the setup.<p>
<br>
  6. Start the Metasploitable machine and let it boot up!<br>
<br>
  Login Credentials for Metasploitable<br>
    Username: msfadmin<br>
    Password: msfadmin<br>
  Once the machine is running, you’re all set to start using it!<br>
<br><br> 

MODULE 1<br>
<br>
**Step 1: FTP (File Transfer Protocol) - Connect Anonymously and List Directories**<br>
A. Identify IP Addresses on Your Network<br>
To begin, use the command 'ifconfig' to check your machine’s network configuration. This will show you the IP addresses of the devices on your network.<br>
<br>
For example, on my network:<br>
  Kali machine: 192.168.64.2<br>
  Metasploitable machine: 192.168.64.4<br>
<br>
B. Scan Target Machine with Nmap<br>
To identify which ports are open on the target machine, use Nmap to scan port 21, which is the standard port for FTP.<br>
  nmap 192.168.64.4 -v -sT -SV -p 21 -O<br>
<img width="1171" alt="FTP1" src="https://github.com/user-attachments/assets/4eb83929-f1a2-4e79-b16d-82020468d3d1" /> <br>
<img width="1028" alt="FTP2" src="https://github.com/user-attachments/assets/f1d5bfa7-a80b-43cc-ac15-b4658b077e9e" /> <br>
-v: Verbose mode, gives detailed results.<br>
-sT: Performs a TCP connect scan.<br>
-SV: Identifies services running on the ports.<br>
-p 21: Scans port 21 (FTP).<br>
-O: Detects the target’s operating system.<br>
<br>
C. Connect to the Target with FTP<br>
Once the Nmap scan shows port 21 is open, you can use the FTP command to connect to the target machine.<br>
  ftp 192.168.64.4<br>
<img width="559" alt="FTP3" src="https://github.com/user-attachments/assets/2c146e96-8c3e-4664-9ca8-3816a3367052" /><br>
This command allows you to access the Metasploitable machine via FTP. You’re connecting anonymously, meaning you don’t need a username or password.<br>
<br><br>

**Step 2: SMB (Server Message Block) - Access Shared Resources**<br>
A. List Accessible Ports with Nmap<br>
Use Nmap to identify all the accessible ports on the target machine and see if SMB services are available.<br>
  nmap -sV 192.168.164.4 -vv<br>
<img width="940" alt="SMB1" src="https://github.com/user-attachments/assets/ae15d95c-3da4-4085-85b4-420b445a9a4a" /><br>
<img width="1208" alt="SMB2" src="https://github.com/user-attachments/assets/13eba281-375c-4282-b37d-3a506f4d80fb" /><br>
-sV: Detects versions of services running on open ports.<br>
-vv: Verbose mode for more detailed information.<br>
<br>
B. List Shared Files Using Enum4Linux<br>
After scanning the ports, use enum4linux to list all the available shared resources on the target machine.<br>
  enum4linux -L -S 192.168.164.4<br>
<img width="1253" alt="SMB3" src="https://github.com/user-attachments/assets/001ec6ad-e589-4df2-80f8-b7c7cb440fdb" /><br>
<img width="1130" alt="SMB4" src="https://github.com/user-attachments/assets/3912cbf7-8c52-4dc7-9cac-34c40bbbb7f3" /><br>
This command queries the target machine for its available shared files, showing you a list of accessible directories and files over SMB.<br>
<br>
C. Access SMB with Metasploit<br>
You can use Metasploit to gain access to shared files. Start the Metasploit console and use the symlink_traversal exploit.<br>
  msfconsole  <br>
Then search for the symlink_traversal module to access shared directories.<br>
<br>
D. Set Rhosts and Exploit<br>
Once you've found the shared file you want to access (e.g., the /tmp directory), set the RHOSTS variable to the target machine’s IP address and the SMBSHARE variable to the shared directory path.<br>
  set RHOSTS 192.168.164.4<br>
  set SMB_SHARE /tmp<br>
  run<br>
<img width="1279" alt="SMB5" src="https://github.com/user-attachments/assets/76b52f14-96f5-4c6d-bbf9-7c70cb879f2c" />  <br>
<img width="1005" alt="SMB_final" src="https://github.com/user-attachments/assets/85dd13f8-5e25-48b7-afa2-301f13d1bb9e" /><br>
<br>
This will exploit the SMB vulnerability and allow you to access the files in the shared directory.<br>
<br><br>
 
**Step 3: Telnet - Connect to Remote Servers**<br>
A. Use Telnet to Connect<br>
Telnet is another tool used for remote access to machines. In Metasploit, you can use the auxiliary/scanner/telnet module to scan and connect to remote Telnet servers.
  msfconsole<br>
  use auxiliary/scanner/telnet<br>
<img width="1321" alt="Telnet1" src="https://github.com/user-attachments/assets/40cfee8e-a747-4fab-840f-e21f840e51c5" /><br>
Once the module is selected, use the show options command to see what needs to be configured, such as the target IP address. Then run the exploit to gain access.<br>
<img width="1291" alt="Telnet2" src="https://github.com/user-attachments/assets/5c22ed47-17f7-4bbc-9ba6-4b281d731631" /><br>
<img width="1319" alt="Telnet_final" src="https://github.com/user-attachments/assets/b52082fa-d81a-4c9d-99fb-2e7da1735eb0" /><br>
<br>
Step 4: Rsync - Sync Files Over SSH<br>
Rsync is a tool that allows you to sync files between remote and local systems over SSH. If you're already familiar with SSH, using rsync will make transferring files seamless and efficient.<br>
  rsync -avz -e ssh /local/folder/ remoteuser@remotehost:/remote/folder/<br>
<br>
Step 5: RDP (Remote Desktop Protocol) - Establish Anonymous RDP Session<br>
To access a Windows machine using RDP (Remote Desktop Protocol), you can use rdesktop or xfreerdp command on Linux. This allows you to connect to a Windows machine and interact with its graphical interface.<br>
<br>
A. Command to Connect Using RDP<br>
xfreerdp /u:[username] /v:[host:port]<br>
<img width="727" alt="RDP1" src="https://github.com/user-attachments/assets/ef8e5255-692c-46d0-8097-5c2d4d402dec" /><br>
Replace [username] with the credentials for the Windows machine.<br>
Replace [host:port] with the IP address and RDP port (default is 3389).<br>
<br>
B. Troubleshoot RDP Connection Issues<br>
If you can’t connect, check the following:<br>
  Ping the target machine to make sure it’s reachable.<br>
  Ensure RDP is enabled on the Windows machine.<br>
  Make sure the firewall (such as Windows Defender) is disabled or configured to allow RDP.<br>
<br><br>

**MODULE 2: PORT SCANNING WITH NMAP**<br>
<br>
Port scanning is an essential part of cybersecurity. It helps you discover open ports, running services, and potential vulnerabilities on a target machine. In this guide, you’ll learn how to use Nmap, a powerful network scanning tool, to identify active ports and analyze their services.<br>
<br>
Step 1: Perform a Basic Nmap Scan<br>
A basic Nmap scan helps you identify which ports are open on a target machine.<br>
  nmap [IP address]<br>
Replace [IP address] with the actual IP address of your target.<br>
<img width="669" alt="nmap1" src="https://github.com/user-attachments/assets/4f89b8f2-4541-4bd6-91b8-0a10ed275be4" /><br>
This command tells Nmap to scan the target and list all open ports along with the associated services.<br>
It provides a general overview of what’s accessible on the system.<br>
    A few port meanings:<br>
    22 (SSH): Secure Shell, used for remote access.<br>
    80 (HTTP): Web traffic, often used for websites.<br>
    443 (HTTPS): Secure web traffic.<br>
    This scan gives you a quick snapshot of which services are exposed.<br>
<br>
Step 2: Try Nmap Scans with Flags<br>
Nmap offers different scanning techniques depending on what information you need.<br>
<br>
TCP SYN Scan (-sS – Stealth Scan)<br>
  nmap -sS [IP address]<br>
<img width="773" alt="nmap2" src="https://github.com/user-attachments/assets/cef4b7fd-e6e3-4e4a-9067-c7a810d93e16" /><br>
This stealth scan tries to detect open ports without establishing a full connection, making it less detectable.<br>
It’s commonly used in penetration testing to avoid detection by firewalls or intrusion detection systems.<br>
<br>
The scan confirms port 21 (FTP), port 22 (SSH), port 80 (HTTP), and port 3306 (MySQL database), among others (23 total) are open.<br>
If MySQL (3306) is open, unauthorized access could be a security risk.<br>
<br>
UDP Scan (-sU – Finding UDP Services)<br>
  nmap -sU [IP address]<br>
<img width="838" alt="nmap3" src="https://github.com/user-attachments/assets/4e3fdb65-9591-4fc2-82ae-9ce23ac9feed" /><br>
UDP services are often overlooked but can include critical services like DNS (53), SNMP (161), or DHCP (67/68).<br>
Unlike TCP, UDP doesn’t establish connections, so this scan may take longer.<br>
<br>
This tells us SNMP (Simple Network Management Protocol) is open, which can be a target for attackers if misconfigured.<br>
If no UDP ports are found, the scan may return:<br>
  All 1000 scanned ports on 192.168.64.4 are closed<br>
This means the machine isn’t running any exposed UDP services, or they are well-secured.<br>
<br>
OS Detection (-O – Identify the Operating System)<br>
  nmap -O [IP address]<br>
<img width="726" alt="nmap4" src="https://github.com/user-attachments/assets/87ed2e14-b8e6-4703-8e52-30d6fc82691d" /><br>
This scan analyzes response behavior to determine the operating system (OS) version running on the target.<br>
Knowing the OS helps in identifying potential vulnerabilities.<br>
<br>
The target is running a Linux system, specifically a version of Ubuntu.<br>
Attackers might search for known exploits related to that OS version.<br>
<br>
Step 3: Analyze Scan Results and Identify Security Risks<br>
After scanning, carefully document the open ports and their associated services.<br>
<img width="729" alt="nmap5" src="https://github.com/user-attachments/assets/5c130b25-cc17-4c3d-a815-7e3017dd8c99" /><br>
Port 21 (FTP) → Used for file transfers. Can be a security risk if anonymous login is enabled.<br>
Port 22 (SSH) → Allows secure remote access. Check if password authentication is enabled (brute-force attacks are common).<br>
Port 80/443 (HTTP/HTTPS) → Used for web traffic. If a vulnerable web app is running, it could be exploited.<br>
Port 3306 (MySQL Database) → If exposed, an attacker could attempt to access the database remotely.<br>
<br>
Final Step: Save Your Findings and Exit<br>
Save the results to a file for documentation.<br>
Close unnecessary open ports to reduce attack risks.<br>

<br><br>
**MODULE 3: LEARN HOW TO CONNECT TO A MONGODB SERVER**<br>
<br>
1. Install MongoDB on Windows<br>
Start your Windows VM and open a browser.<br>
Go to MongoDB Community Download - https://www.mongodb.com/try/download/community<br>
Choose the Windows MSI package (latest version) and download it.<br>
Run the installer and select Complete Installation to get all features.<br>
Finish the installation.<br>
<br>
<i>Important Note:
The traditional mongo command doesn't work on Windows 10+. Use mongosh (MongoDB Shell) instead. </i><br>
<br>
2. Add MongoDB to System Path (Optional but Recommended)<br>
By default, MongoDB commands won't work from any location in the terminal. Adding it to the system path fixes this.<br>
<br>
Find MongoDB’s installation folder (typically):<br>
  C:\Program Files\MongoDB\Server\8.0\bin<br>
Add this path to the system environment variables:<br>
Search for "Environment Variables" in Windows.<br>
Click Edit the system environment variables → Advanced tab → Environment Variables.<br>
Under User Variables, find and edit Path.<br>
Click New, paste the path, and save changes.<br>
Restart your PowerShell or Command Prompt for changes to apply.<br>
<br>
3. Start MongoDB and Connect to the Server<br>
Open PowerShell or Command Prompt.<br>
  mongosh<br>
If everything is set up correctly, you’ll see a MongoDB shell prompt confirming the connection.<br>
To check the installed version, run:<br>
  db.version()<br>
Troubleshooting:<br>
If you get a connection error, ensure MongoDB is running. You can manually start it using:<br>
  net start MongoDB<br>
<img width="856" alt="mongodb" src="https://github.com/user-attachments/assets/01367139-fd8f-460a-a116-22d56845a6d5" /><br>
<br>
4. Explore Databases and Identify Security Risks<br>
Once connected, you can start investigating MongoDB for vulnerabilities.<br>
<br>
List Available Databases<br>
  show dbs<br>
If you can see multiple databases without authentication, the server is misconfigured and exposed.<br>
<br>
Access a Specific Database<br>
  use <database-name><br>
For example:<br>
  use testDB<br>
List Collections (Tables)<br>
  show collections<br>
Check for Sensitive Data<br>
View documents inside a collection:<br>
  db.<collection-name>.find().limit(5).pretty()<br>
If personal or sensitive information is accessible without login credentials, the database lacks security controls.<br>
<br>
Look for Misconfigurations<br>
Check if user credentials are stored in plain text:<br>
  use admin<br>
  db.system.users.find()<br>
If this command returns user accounts and passwords, the server is highly vulnerable.<br>
<br>
5. Disconnect from MongoDB<br>
When you're done, exit the shell by typing:<br>
  exit<br>
<br>

**MODULE 4: Practical Application – Hack the Box (HTB)**
How to Set Up the Environment

1. Open the machine dropdown menu:
    In the menu, select "Connect using Pwnbox". This will open a new tab for you to choose a server.
   
3. Choose a nearby server:
    To reduce lag, pick a server that is geographically closer to you. This will ensure faster connections.
   
5. Click “START PWNBOX”:
    Once you’ve selected the server, click the "START PWNBOX" button to start the virtual machine.

4. Wait for the virtual machine to be online:
    After a few moments, your virtual machine should be up and running. When it's ready, click "OPEN DESKTOP" to open a new tab where you can interact with the virtual       machine.

5. Spawn the target machine:
    In the dropdown menu, find and click "SPAWN MACHINE" for the specific machine you want to use (e.g., "Meow").
    Take note of the machine's IP address — you'll need this to interact with it.

6. Open a terminal:
    Click on the green console icon (it’s at the top middle of the screen) to open a terminal window.

7. Test the connection:
    In the terminal, type the following command to test the connection to the target machine:
      ping <target-IP>
      Replace <target-IP> with the actual IP address of the machine.
   
8. Verify the connection:
    If you receive responses from the target machine (like "Reply from..."), it means you're connected and ready to start penetration testing.

The initial assessment (Nmap scan, services found).
Steps taken to exploit vulnerabilities and gain access.
Final shell access and the flag(s) captured.


Hack the Box: Meow (Telnet)

1. Ping the machine to check connectivity:
  Open a terminal and type the following command:

    ping 10.129.215.115
  This checks if the machine is reachable and ensures your network connection is active. If you get responses like "Reply from 10.129.215.115", the machine is online and   ready to interact with.

2. Scan for open ports using Nmap:
  Next, perform a port scan to see which services are running on the target machine by typing:

    sudo nmap -sV 10.129.215.115
    This command scans the machine for open ports and tries to identify the services running on those ports. Here’s what you might see:
  <img width="958" alt="HTB_telnet1" src="https://github.com/user-attachments/assets/f7aeaa39-b284-4295-8e86-95ed511f75a3" />
  In this case, port 23 (Telnet) is open, meaning we can try to access the machine through Telnet.

3. Access the machine using Telnet:
  To connect to the machine via Telnet, use the following command:

    telnet 10.129.215.115
   <img width="809" alt="HTB_telnet2" src="https://github.com/user-attachments/assets/b6b3b92a-0d52-45b7-a873-3714e4454ad8" />
  You will be prompted to log in. Try common usernames like administrator, admin, and root. In this example, we try the following:
  Once we try root, we gain access to the machine.

4. Explore the files on the machine:
  Now that you have access as root, you can list the files on the system. To do this, type:
    ls
  This will show the files and directories in the current location. You should see a file, such as a flag text file.

5. View the flag:
  To read the contents of the flag file, use the cat command:
    cat flag.txt
   
In this example, the flag text file content is:
    b40abdfe23665f766f9c61ecba8a4c19
<img width="806" alt="HTB_telnet3" src="https://github.com/user-attachments/assets/a66a49de-420d-46cd-9403-5b4991f9fc48" />
This is the flag we were looking for!

Hack the Box: Fawn (FTP)

1. Ping the machine to check connectivity:
  First, you need to check if the target machine is reachable. Open a terminal and run the following command:

    ping 10.129.206.86
    This command sends a signal to the target machine and waits for a response. If you receive replies like "Reply from 10.129.206.86", it means the machine is online         and you have network connectivity.

2. Scan for open ports using Nmap:
  Now, let's scan the target machine for open ports. Type the following command to see what services are running on which ports:

    sudo nmap -sV 10.129.206.86
    This Nmap command checks for open ports and attempts to identify the services running on them. This step helps you gather information about the target machine and        decide which service to target for exploitation.

3. Install and use FTP to exploit the machine:
  FTP (File Transfer Protocol) is a common service that can have misconfigurations, such as allowing anonymous access.
  First, make sure FTP is installed on your system by running:

    sudo apt-get install ftp
  Once it's installed, you can use FTP to connect to the target machine. Run this command:

    ftp 10.129.206.86
  <img width="834" alt="HTB_ftp1" src="https://github.com/user-attachments/assets/391d77ae-de9e-4e3c-8821-97a263415cb6" />

  Important note: Some FTP servers may have an anonymous login feature, which means you can log in without a username or password. If the server is misconfigured, you can log in using any password.

4. List the files on the machine:
  After successfully logging in, you can list the files and directories on the target machine using the ls command:

  This will display all the files in the current directory. You should see a file named flag.txt, which is usually the file you're after in penetration testing exercises.

5. Download the flag file:
  Now that you've found the flag.txt file, use the get command to download it to your local machine and view it:

    get flag.txt
   <img width="833" alt="HTB_ftp2" src="https://github.com/user-attachments/assets/f4afb235-2260-4eb9-a3f0-a3921396c6b4" />
  This command transfers the flag.txt file from the target machine to your local machine, where you can view its contents.
   
7. Additional FTP commands:

To see a list of available commands for FTP, use the help command while in the FTP session:
    help
  If you're unsure about FTP's capabilities or need to check its options, you can run:
    ftp -?
  This will display the FTP command options and provide you with useful information for interacting with the service.
          
Hack the Box: Dancing (SMB)

1. Check if the Target Machine is Reachable (Ping Test)
  Before attempting any actions, ensure the machine is online by sending a ping request:
    ping 10.129.172.28
   If the machine responds, you're connected and can proceed. If not, check your network settings or ensure the target is powered on.

2. Scan for Open Ports with Nmap
  Now, let's scan the target for open ports and running services using Nmap:
    sudo nmap -sV 10.129.172.28
      sudo: Runs the command with administrator privileges.
      nmap: Network scanning tool.
      -sV: Detects service versions running on open ports.
  This scan will show which ports are open and what services are running on them.

3. Identify Available SMB Shares
  The scan results reveal available SMB (Server Message Block) shares on the target machine. Common shares include:
    ADMIN$: A hidden administrative share.
    C$: The root of the C: drive (default administrative share).
    IPC$: Used for interprocess communication.
    WorkShares: A user-defined shared folder.

4. Attempt to Access SMB Shares
  Now, we try to connect to the shared folders using the smbclient command:

    smbclient \\\\10.129.172.28\\WorkShares
   <img width="831" alt="HTB_smb1" src="https://github.com/user-attachments/assets/c3bfe13e-16da-42ca-a49b-58ad3b439779" />
  Breaking down the command:
      smbclient: Tool to interact with SMB shares.
      \\\\10.129.172.28\\WorkShares: Specifies the target machine and the share name.
  If prompted for a password, try entering nothing (some misconfigured shares allow anonymous access).

5. List Available Files and Directories
  Once inside the share, use the ls command to view its contents:
    ls
   <img width="828" alt="HTB_smb2" src="https://github.com/user-attachments/assets/173639ae-bd25-40e2-b4d1-5196220e80b6" />
  This will list all the files and folders available within WorkShares.

6. Explore Users’ Directories (Amy and James)
  Navigate through the directories to find useful files.
  In James' directory, you'll find flag.txt—a key file for this exercise.

7. Download the Flag File
  To retrieve flag.txt, use the get command:
    get flag.txt
  This downloads the file from the target machine to your local machine.

8. View the Flag File Contents
  Open and read the flag file using:
    cat flag.txt
   <img width="824" alt="HTB_smb3" src="https://github.com/user-attachments/assets/b66424ae-23ac-4230-a070-75794e1583f9" />
  This will display the file's content, which is usually a proof-of-concept flag in penetration testing exercises.


Hack The Box: Redeemer (Redis)

1. Check if the Target Machine is Reachable (Ping Test)
  Before scanning for services, make sure the target machine is online:
    ping 10.129.252.111
  If the machine responds, you're good to go! If not, check your network connection or verify that the machine is running.

2. Scan for Open Ports with Nmap
  Now, let's scan the target for open ports and running services:
    sudo nmap -sV 10.129.252.111
      sudo: Runs the command with administrator privileges.
      nmap: A tool for scanning network devices.
      -sV: Detects the versions of services running on open ports.
      The scan shows that port 6379 is open, indicating that a Redis key-value store (version 5.0.7) is running.
  
3. Install Redis CLI (If Not Installed)
  To interact with Redis, we need redis-cli. If it's not installed, install it with:
    sudo apt install redis-tools
  This tool allows us to connect to Redis and execute commands.

4. Explore Redis CLI Commands
  To see available Redis commands, use:
    redis-cli --help
   <img width="1008" alt="HTB_redis1" src="https://github.com/user-attachments/assets/585b9727-a10d-44a3-96dd-ba78d2a2ee0a" />
  This will display useful options and syntax for interacting with the database.

5. Connect to the Redis Server
  Use the following command to connect:
    redis-cli -h 10.129.252.111
      redis-cli: Launches the Redis command-line tool.
      -h: Specifies the target host (the Redis server).
  If the connection is successful, you'll see a Redis prompt (e.g., 127.0.0.1:6379>), indicating that you're inside the database.

6. Enumerate Redis Information
  The info command provides details about the Redis server, including memory usage, connected clients, and database statistics:
    info
  Look for the Keyspace section, which shows:
    The number of databases.
    The number of keys stored in each database.
    Expiration settings for keys.
  In this case, the scan shows one database (index 0).

7. Select the Redis Database
  Since only one database exists, select it using:
    select 0
  This ensures we are working in the correct database.

8. List Available Keys
  To see all stored keys in the database, use:
    keys *
  This command retrieves all keys stored in the Redis database. Keys can contain valuable data such as credentials or configurations.

9. Download or Access Keys
  Once the keys are listed, retrieve their values using:
    get <key_name>
    <img width="805" alt="HTB_redis2" src="https://github.com/user-attachments/assets/333bdf92-386b-4b1b-97d7-824b312d9640" />
  Replace <key_name> with the actual key you want to view.

  
