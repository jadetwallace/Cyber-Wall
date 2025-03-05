# Cyber-Wall

VIRTUAL MACHINE & TOOL SETUP\
Before starting, make sure you have:\

A virtualized environment (like VirtualBox, UTM or VMware). I used UTM because I’m on a Mac.\
Kali Linux (either the ISO or pre-installed version).\
A local test environment with your target system (e.g., Metasploitable).\
\
How to Set Up UTM (for Mac)<p>
  Go to the UTM website: https://mac.getutm.app/<p>
  Download UTM and follow the installation instructions.<p>
<p>
How to Set Up Kali Linux in UTM<p>
  1. Download Kali Linux from https://www.kali.org/:<p>
    Click "Download".<p>
    Under "Installer images", select and download the 64-bit version.<p>
<p>
  2. Open UTM and click Create new virtual machine.<p>
    Choose Virtualize.<p>
    Select Linux (OS).<p>
    Browse and select the Kali Linux ISO file, then click Continue.<p>
  <p>
  3. Choose your hardware and storage preferences.<p>
    Title your Kali machine.<p>
  <p>
  4. Start the Kali machine:<p>
    Select Graphical install and follow the installation prompts.<p>
    The installation process may take a few minutes.<p>
    After it finishes, shut down Kali.<p>
  <p>
  5. Right-click the Kali machine and click Edit.<p>
    Scroll to find the IDE Drive associated with the ISO and delete it.<p>
  <p>
  6. Start the Kali machine, and you’re ready to go!<p>
<p>
How to Set Up Metasploitable 2<p>
  1. Download Metasploitable 2 from this link and expand the zip file.<p>
<p>
  2. Convert the .vmdk file to .qcow2 format:<p>
    Open Terminal and install Qemu:<p>
      brew install qemu<p>
    Navigate to the folder where you downloaded Metasploitable 2:<p>
      cd Downloads<p>
      cd Metasploitable2-Linux<p>
    Convert the file using this command:<p>
      qemu-img convert -p -O qcow2 'Metasploitable.vmdk' Metasploitable.qcow2<p>
 <p>    
  3. Open UTM and click Create new virtual machine.<p>
    Select Emulate and choose Other.<p>
    Choose None for Boot Device.<p>
    Set memory to 512 MB and storage to 10 GB.<p>
    Continue until you reach the Title page, and give your Metasploitable machine a name.<p>
<p>
  4. Right-click the Metasploitable machine and click Edit.<p>
    Select QEMU on the left.<p>
    Deselect UEFI Boot.<p>
    Scroll to the IDE Drive and delete it.<p>
<p>
  5. Click New to create a new drive.<p>
    Choose Import and navigate to the qcow2 file you created earlier.<p>
    Select it and finish the setup.<p>
<p>
  6. Start the Metasploitable machine and let it boot up!<p>
<p>
  Login Credentials for Metasploitable<p>
    Username: msfadmin<p>
    Password: msfadmin<p>
  Once the machine is running, you’re all set to start using it!<p>
<p><p>  

MODULE 1<p>
**Step 1: FTP (File Transfer Protocol) - Connect Anonymously and List Directories**<p>
A. Identify IP Addresses on Your Network<p>
To begin, use the command 'ifconfig' to check your machine’s network configuration. This will show you the IP addresses of the devices on your network.<p>
<p>
For example, on my network:<p>
  Kali machine: 192.168.64.2<p>
  Metasploitable machine: 192.168.64.4<p>
<p>
B. Scan Target Machine with Nmap<p>
To identify which ports are open on the target machine, use Nmap to scan port 21, which is the standard port for FTP.<p>
  nmap 192.168.64.4 -v -sT -SV -p 21 -O<p>
<img width="1171" alt="FTP1" src="https://github.com/user-attachments/assets/4eb83929-f1a2-4e79-b16d-82020468d3d1" /> <p>
<img width="1028" alt="FTP2" src="https://github.com/user-attachments/assets/f1d5bfa7-a80b-43cc-ac15-b4658b077e9e" /> <p>
-v: Verbose mode, gives detailed results.<p>
-sT: Performs a TCP connect scan.<p>
-SV: Identifies services running on the ports.<p>
-p 21: Scans port 21 (FTP).<p>
-O: Detects the target’s operating system.<p>
<p>
C. Connect to the Target with FTP<p>
Once the Nmap scan shows port 21 is open, you can use the FTP command to connect to the target machine.<p>
  ftp 192.168.64.4<p>
<img width="559" alt="FTP3" src="https://github.com/user-attachments/assets/2c146e96-8c3e-4664-9ca8-3816a3367052" /><p>
This command allows you to access the Metasploitable machine via FTP. You’re connecting anonymously, meaning you don’t need a username or password.<p>


**Step 2: SMB (Server Message Block) - Access Shared Resources**
A. List Accessible Ports with Nmap
Use Nmap to identify all the accessible ports on the target machine and see if SMB services are available.
  nmap -sV 192.168.164.4 -vv
<img width="940" alt="SMB1" src="https://github.com/user-attachments/assets/ae15d95c-3da4-4085-85b4-420b445a9a4a" />
<img width="1208" alt="SMB2" src="https://github.com/user-attachments/assets/13eba281-375c-4282-b37d-3a506f4d80fb" />
-sV: Detects versions of services running on open ports.
-vv: Verbose mode for more detailed information.

B. List Shared Files Using Enum4Linux
After scanning the ports, use enum4linux to list all the available shared resources on the target machine.
  enum4linux -L -S 192.168.164.4
<img width="1253" alt="SMB3" src="https://github.com/user-attachments/assets/001ec6ad-e589-4df2-80f8-b7c7cb440fdb" />
<img width="1130" alt="SMB4" src="https://github.com/user-attachments/assets/3912cbf7-8c52-4dc7-9cac-34c40bbbb7f3" />
This command queries the target machine for its available shared files, showing you a list of accessible directories and files over SMB.

C. Access SMB with Metasploit
You can use Metasploit to gain access to shared files. Start the Metasploit console and use the symlink_traversal exploit.
  msfconsole  
Then search for the symlink_traversal module to access shared directories.

D. Set Rhosts and Exploit
Once you've found the shared file you want to access (e.g., the /tmp directory), set the RHOSTS variable to the target machine’s IP address and the SMBSHARE variable to the shared directory path.
  set RHOSTS 192.168.164.4
  set SMB_SHARE /tmp
  run
<img width="1279" alt="SMB5" src="https://github.com/user-attachments/assets/76b52f14-96f5-4c6d-bbf9-7c70cb879f2c" />  
<img width="1005" alt="SMB_final" src="https://github.com/user-attachments/assets/85dd13f8-5e25-48b7-afa2-301f13d1bb9e" />

This will exploit the SMB vulnerability and allow you to access the files in the shared directory.

 
**Step 3: Telnet - Connect to Remote Servers**
A. Use Telnet to Connect
Telnet is another tool used for remote access to machines. In Metasploit, you can use the auxiliary/scanner/telnet module to scan and connect to remote Telnet servers.
  msfconsole
  use auxiliary/scanner/telnet
<img width="1321" alt="Telnet1" src="https://github.com/user-attachments/assets/40cfee8e-a747-4fab-840f-e21f840e51c5" />
Once the module is selected, use the show options command to see what needs to be configured, such as the target IP address. Then run the exploit to gain access.
<img width="1291" alt="Telnet2" src="https://github.com/user-attachments/assets/5c22ed47-17f7-4bbc-9ba6-4b281d731631" />
<img width="1319" alt="Telnet_final" src="https://github.com/user-attachments/assets/b52082fa-d81a-4c9d-99fb-2e7da1735eb0" />

Step 4: Rsync - Sync Files Over SSH
Rsync is a tool that allows you to sync files between remote and local systems over SSH. If you're already familiar with SSH, using rsync will make transferring files seamless and efficient.
  rsync -avz -e ssh /local/folder/ remoteuser@remotehost:/remote/folder/

Step 5: RDP (Remote Desktop Protocol) - Establish Anonymous RDP Session
To access a Windows machine using RDP (Remote Desktop Protocol), you can use rdesktop or xfreerdp command on Linux. This allows you to connect to a Windows machine and interact with its graphical interface.

A. Command to Connect Using RDP
xfreerdp /u:[username] /v:[host:port]
<img width="727" alt="RDP1" src="https://github.com/user-attachments/assets/ef8e5255-692c-46d0-8097-5c2d4d402dec" />
Replace [username] with the credentials for the Windows machine.
Replace [host:port] with the IP address and RDP port (default is 3389).

B. Troubleshoot RDP Connection Issues
If you can’t connect, check the following:
  Ping the target machine to make sure it’s reachable.
  Ensure RDP is enabled on the Windows machine.
  Make sure the firewall (such as Windows Defender) is disabled or configured to allow RDP.


**MODULE 2: PORT SCANNING WITH NMAP**

Port scanning is an essential part of cybersecurity. It helps you discover open ports, running services, and potential vulnerabilities on a target machine. In this guide, you’ll learn how to use Nmap, a powerful network scanning tool, to identify active ports and analyze their services.

Step 1: Perform a Basic Nmap Scan
A basic Nmap scan helps you identify which ports are open on a target machine.
  nmap [IP address]
Replace [IP address] with the actual IP address of your target.
<img width="669" alt="nmap1" src="https://github.com/user-attachments/assets/4f89b8f2-4541-4bd6-91b8-0a10ed275be4" />
This command tells Nmap to scan the target and list all open ports along with the associated services.
It provides a general overview of what’s accessible on the system.
    A few port meanings:
    22 (SSH): Secure Shell, used for remote access.
    80 (HTTP): Web traffic, often used for websites.
    443 (HTTPS): Secure web traffic.
    This scan gives you a quick snapshot of which services are exposed.

Step 2: Try Nmap Scans with Flags
Nmap offers different scanning techniques depending on what information you need.

TCP SYN Scan (-sS – Stealth Scan)
  nmap -sS [IP address]
<img width="773" alt="nmap2" src="https://github.com/user-attachments/assets/cef4b7fd-e6e3-4e4a-9067-c7a810d93e16" />
This stealth scan tries to detect open ports without establishing a full connection, making it less detectable.
It’s commonly used in penetration testing to avoid detection by firewalls or intrusion detection systems.

The scan confirms port 21 (FTP), port 22 (SSH), port 80 (HTTP), and port 3306 (MySQL database), among others (23 total) are open.
If MySQL (3306) is open, unauthorized access could be a security risk.

UDP Scan (-sU – Finding UDP Services)
  nmap -sU [IP address]
<img width="838" alt="nmap3" src="https://github.com/user-attachments/assets/4e3fdb65-9591-4fc2-82ae-9ce23ac9feed" />
UDP services are often overlooked but can include critical services like DNS (53), SNMP (161), or DHCP (67/68).
Unlike TCP, UDP doesn’t establish connections, so this scan may take longer.

This tells us SNMP (Simple Network Management Protocol) is open, which can be a target for attackers if misconfigured.
If no UDP ports are found, the scan may return:
  All 1000 scanned ports on 192.168.64.4 are closed
This means the machine isn’t running any exposed UDP services, or they are well-secured.

OS Detection (-O – Identify the Operating System)
  nmap -O [IP address]
<img width="726" alt="nmap4" src="https://github.com/user-attachments/assets/87ed2e14-b8e6-4703-8e52-30d6fc82691d" />
This scan analyzes response behavior to determine the operating system (OS) version running on the target.
Knowing the OS helps in identifying potential vulnerabilities.

The target is running a Linux system, specifically a version of Ubuntu.
Attackers might search for known exploits related to that OS version.

Step 3: Analyze Scan Results and Identify Security Risks
After scanning, carefully document the open ports and their associated services.
<img width="729" alt="nmap5" src="https://github.com/user-attachments/assets/5c130b25-cc17-4c3d-a815-7e3017dd8c99" />
Port 21 (FTP) → Used for file transfers. Can be a security risk if anonymous login is enabled.
Port 22 (SSH) → Allows secure remote access. Check if password authentication is enabled (brute-force attacks are common).
Port 80/443 (HTTP/HTTPS) → Used for web traffic. If a vulnerable web app is running, it could be exploited.
Port 3306 (MySQL Database) → If exposed, an attacker could attempt to access the database remotely.

Final Step: Save Your Findings and Exit
Save the results to a file for documentation.
Close unnecessary open ports to reduce attack risks.


**MODULE 3: LEARN HOW TO CONNECT TO A MONGODB SERVER**

1. Install MongoDB on Windows
Start your Windows VM and open a browser.
Go to MongoDB Community Download - https://www.mongodb.com/try/download/community
Choose the Windows MSI package (latest version) and download it.
Run the installer and select Complete Installation to get all features.
Finish the installation.

<i>Important Note:
The traditional mongo command doesn't work on Windows 10+. Use mongosh (MongoDB Shell) instead. </i>

2. Add MongoDB to System Path (Optional but Recommended)
By default, MongoDB commands won't work from any location in the terminal. Adding it to the system path fixes this.

Find MongoDB’s installation folder (typically):
  C:\Program Files\MongoDB\Server\8.0\bin
Add this path to the system environment variables:
Search for "Environment Variables" in Windows.
Click Edit the system environment variables → Advanced tab → Environment Variables.
Under User Variables, find and edit Path.
Click New, paste the path, and save changes.
Restart your PowerShell or Command Prompt for changes to apply.

3. Start MongoDB and Connect to the Server
Open PowerShell or Command Prompt.
  mongosh
If everything is set up correctly, you’ll see a MongoDB shell prompt confirming the connection.
To check the installed version, run:
  db.version()
Troubleshooting:
If you get a connection error, ensure MongoDB is running. You can manually start it using:
  net start MongoDB
<img width="856" alt="mongodb" src="https://github.com/user-attachments/assets/01367139-fd8f-460a-a116-22d56845a6d5" />

4. Explore Databases and Identify Security Risks
Once connected, you can start investigating MongoDB for vulnerabilities.

List Available Databases
  show dbs
If you can see multiple databases without authentication, the server is misconfigured and exposed.

Access a Specific Database
  use <database-name>
For example:
  use testDB
List Collections (Tables)
  show collections
Check for Sensitive Data
View documents inside a collection:
  db.<collection-name>.find().limit(5).pretty()
If personal or sensitive information is accessible without login credentials, the database lacks security controls.

Look for Misconfigurations
Check if user credentials are stored in plain text:
  use admin
  db.system.users.find()
If this command returns user accounts and passwords, the server is highly vulnerable.

5. Disconnect from MongoDB
When you're done, exit the shell by typing:
  exit


**Set up a MongoDB server or find a vulnerable MongoDB server to connect to**
We're going to set up the MongoDB Server from with the Windows VM.
  Once the Windows VM is running, in a browser, go to https://www.mongodb.com/try/download/community, select the appropriate version for the machine you're running and download the msi package.
  After the download is complete, click through the installation process. It is recommended to install the 'Complete' version, however, please note that the 'mongo' command will not work on Windows 10 and the Shell version will need to be downloaded.
  Also, if using the Windows installation, we would have to access the full path, to avoid this we can set the Path.
    Go to the path to the bin folder, which will look similar to the following: 
      C:\Program Files\MongoDB\Server\8.0\bin
      In the search bar on the desktop, type 'env' and select Edit the system environment variables
      On the Advanced tab, click on Environment Variables
      Within the User Variables for <username> section, select the path 
      Click on the Edit... button, then New and paste or type in the path> C:\Program Files\MongoDB\Server\8.0\bin
      Click on the Ok to confirm when exiting the settings and restart the Powershell window
      
**Use MongoDB tools like mongo to establish a connection to the server**
  Start the  'mongosh' to execute
  
**Explore the database by listing available databases and checking for any vulnerabilities**


MODULE 4: Practical Application – Hack the Box (HTB)
**Set up**
Open the machine dropdown menu and select "Connect using Pwnbox"
Choose a nearby server for lower latency and click "START PWNBOX"
Once the virtual machine is online, click "OPEN DESKTOP"
In the dropdown menu, click "SPAWN MACHINE" for all the machines (Meow in this case) and note its IP address.
Click the green console icon at the top middle of the screen to open a terminal.
You can test the connection by running:
  ping <target-IP>
If you receive responses, you're all set to start penetration testing.

  
          
          

  
