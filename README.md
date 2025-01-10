<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Setup and Installation Guide</h1>
This guide provides the steps to install and set up the open-source help desk ticketing system, osTicket.<br />

<h2>Technologies and Tools Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Protocol (RDP)
- Internet Information Services (IIS)

<h2>Operating System</h2>

- Windows 10 (22H2)

<h2>Prerequisites</h2>

- An active Microsoft Azure account
- Basic understanding of IIS
- Access to Remote Desktop
- osTicket installation files
- HeidiSQL

<h2>Installation Process</h2>

<h3>Step 1: Create a Virtual Machine</h3>

In Microsoft Azure, set up a new VM in a resource group called "osTicket".

- **VM Name:** osticket-vm  
- **Image:** Windows 10 Pro, version 22H2 - x64 Gen2  
- **Size:** 2 vCPUs, 8 GiB memory  

Ensure you check the licensing box and proceed to create the VM. No changes are needed for management, disks, or networking sections.

<p>
<img src="https://i.imgur.com/Bz829jL.png" height="80%" width="80%" alt="Creating VM"/>
</p>

<p>
<img src="https://i.imgur.com/rHmcRf3.png" height="80%" width="80%" alt="Creating VM"/>
</p>

<h3>Step 2: Access the Virtual Machine</h3>

- Use **Remote Desktop** to log into the VM with the credentials you created.

<p>
<img src="https://i.imgur.com/8IdvRmZ.png" height="80%" width="80%" alt="RDP Access"/>
</p>

<h3>Step 3: Download and Unzip Installation Files</h3>

- Inside the VM, download the `osTicket-Installation-Files.zip` and extract it to your desktop. The extracted folder should be named `osTicket-Installation-Files`.

<p>
<img src="https://i.imgur.com/6imV7Hy.png" height="80%" width="80%" alt="Download and Unzip"/>
</p>

<h3>Step 4: Install IIS and Enable CGI</h3>

- Go to **Control Panel** -> **Programs** -> **Turn Windows features on or off**.
- Enable **IIS** with the following features:
  - **World Wide Web Services** -> **Application Development Features** -> [X] CGI

<p>
<img src="https://i.imgur.com/Htr4j9h.png" height="80%" width="80%" alt="Install IIS"/>
</p>

<p>
<img src="https://i.imgur.com/4Q1PaOl.png" height="80%" width="80%" alt="Enable CGI"/>
</p>

<p>
<img src="https://i.imgur.com/cm20H8J.png" height="80%" width="80%" alt="Enable CGI"/>
</p>

<h3>Step 5: Install Required Components</h3>

- From the `osTicket-Installation-Files` folder:
  - Install **PHP Manager for IIS** using `PHPManagerForIIS_V1.5.0.msi`.
  - Install **Rewrite Module** using `rewrite_amd64_en-US.msi`.

<p>
<img src="https://i.imgur.com/TmRwTh9.png" height="80%" width="80%" alt="Install Components"/>
</p>

<h3>Step 6: Setup PHP</h3>

- Create the directory `C:\PHP`.
- Extract `PHP 7.3.8` (`php-7.3.8-nts-Win32-VC15-x86.zip`) into the `C:\PHP` folder.
- Install `VC_redist.x86.exe`.

<p>
<img src="https://i.imgur.com/Khwf0Tv.png" height="80%" width="80%" alt="Setup PHP"/>
</p>

<p>
<img src="https://i.imgur.com/0IRX6FM.png" height="80%" width="80%" alt="Setup PHP"/>
</p>

<h3>Step 7: Install MySQL</h3>

- From the `osTicket-Installation-Files` folder, install MySQL 5.5.62 (`mysql-5.5.62-win32.msi`).
  - Choose **Typical Setup**.
  - Launch the Configuration Wizard:
    - Choose **Standard Configuration**
    - Set a username and password (e.g., root/root)

<p>
<img src="https://i.imgur.com/m5NO1HX.png" height="80%" width="80%" alt="Install MySQL"/>
</p>

<h3>Step 8: Configure IIS</h3>

- Open IIS as an administrator.
- Register PHP:
  - Go to **PHP Manager** -> Register PHP path -> `C:\PHP\php-cgi.exe`.
- Reload IIS by stopping and starting the server.

<p>
<img src="https://i.imgur.com/m5NO1HX.png" height="80%" width="80%" alt="Configure IIS"/>
</p>

<p>
<img src="https://i.imgur.com/OPR6ELG.png" height="80%" width="80%" alt="Configure IIS"/>
</p>

<h3>Step 9: Install osTicket</h3>

- From the `osTicket-Installation-Files` folder:
  - Extract `osTicket-v1.15.8.zip`.
  - Copy the `upload` folder to `C:\inetpub\wwwroot`.
  - Rename the `upload` folder to `osTicket`.
- Reload IIS by stopping and starting the server.

<p>
<img src="https://i.imgur.com/QAGjmly.png" height="80%" width="80%" alt="Install osTicket"/>
</p>

<p>
<img src="https://i.imgur.com/UNeiP4j.png" height="80%" width="80%" alt="Install osTicket"/>
</p>

<h3>Step 10: Configure osTicket</h3>

- Open IIS:
  - Go to **Sites** -> **Default** -> **osTicket**.
  - On the right, click **Browse *:80**.

<p>
<img src="https://i.imgur.com/Vo85YbB.png" height="80%" width="80%" alt="Configure osTicket"/>
</p>

<p>
<img src="https://i.imgur.com/oM0muFz.png" height="80%" width="80%" alt="Configure osTicket"/>
</p>

- Enable required PHP extensions:
  - Go back to IIS, **Sites** -> **Default** -> **osTicket**.
  - Double-click **PHP Manager** -> Click **Enable or disable an extension**.
  - Enable: `php_imap.dll`, `php_intl.dll`, `php_opcache.dll`.

<p>
<img src="https://i.imgur.com/3w4E5N7.png" height="80%" width="80%" alt="Enable Extensions"/>
</p>

<h3>Step 11: Update Configuration Files</h3>

- Rename `ost-config.php`:
  - From: `C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php`
  - To: `C:\inetpub\wwwroot\osTicket\include\ost-config.php`.
- Assign Permissions:
  - Disable inheritance -> Remove all permissions.
  - Add new permissions -> **Everyone** -> **Full control**.

<p>
<img src="https://i.imgur.com/18W6jYt.png" height="80%" width="80%" alt="Update Config Files"/>
</p>

<p>
<img src="https://i.imgur.com/Gj686F9.png" height="80%" width="80%" alt="Update Config Files"/>
</p>

<h3>Step 12: Complete osTicket Setup</h3>

- In the browser, continue the osTicket setup:
  - Set **Helpdesk Name**.
  - Set **Default email** (receives emails from customers).

<p>
<img src="https://i.imgur.com/lFfXfPa.png" height="80%" width="80%" alt="Complete Setup"/>
</p>

<h3>Step 13: Install HeidiSQL and Configure Database</h3>

- From the `osTicket-Installation-Files` folder, install **HeidiSQL**.
- Open HeidiSQL:
  - Create a new session with username: root and password: root.
  - Connect to the session.
  - Create a database named `osTicket`.

<p>
<img src="https://i.imgur.com/3MktR5j.png" height="80%" width="80%" alt="Install HeidiSQL"/>
</p>

<p>
<img src="https://i.imgur.com/45BnPbc.png" height="80%" width="80%" alt="Configure Database"/>
</p>

<h3>Step 14: Finalize osTicket Installation</h3>

- In the browser, complete the setup:
  - **MySQL Database:** osTicket  
  - **MySQL Username:** root  
  - **MySQL Password:** root  
- Click **Install Now!**

<p>
<img src="https://i.imgur.com/niqOpoY.png" height="80%" width="80%" alt="Finalize Installation"/>
</p>

<h3>Step 15: Verify Installation</h3>

- Access your help desk login page: `http://localhost/osTicket/scp/login.php`.

<p>
<img src="https://i.imgur.com/fsadUTz.png" height="80%" width="80%" alt="Verify Installation"/>
</p>

<h2>Conclusion</h2>

Great job! You have successfully installed and configured osTicket on your virtual machine. Your help desk system is now ready to use!
