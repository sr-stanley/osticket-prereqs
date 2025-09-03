# osticket-prereqs
<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Prerequisites and Installation</h1>
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How To Install osTicket with Prerequisites](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> (22H2)

## Table of Contents
- STEP 1 – Enable IIS with CGI
- STEP 2 – Install PHP and IIS Modules
- STEP 3 – Install Microsoft Visual C++ Redistributable (x86, 2015–2022)
- STEP 4 – Install MySQL 5562
- STEP 5 – Configure PHP in IIS
- STEP 6 – Install and Configure osTicket v1158
- STEP 7 – Install and Configure HeidiSQL

<h2>Installation Steps</h2>

<h3>STEP 1 – Enable IIS with CGI</h3>

**Why:**  
IIS (Internet Information Services) is Microsoft’s web server platform. Enabling **CGI (Common Gateway Interface)** allows IIS to run certain server-side scripts that osTicket requires. Without IIS, your system cannot host the osTicket web application.  

**Instructions:**  
1. Open **Control Panel** → Programs → Programs and Features.  
2. Click **Turn Windows features on or off**.  
3. Enable **Internet Information Services**.  
4. Expand:  
   - **World Wide Web Services** → **Application Development Features** → check **CGI**.  
5. Click **OK** to apply.

<p>
<img width="1315" height="791" alt="1  Enable IIS with CGI" src="https://github.com/user-attachments/assets/2d3b924f-0941-4af7-b963-c02989db64f4" />
</p>

---

<h3>STEP 2 – Install PHP and IIS Modules</h3>

**Why:**  
- **PHP Manager:** Integrates PHP with IIS, ensuring scripts execute correctly.  
- **IIS URL Rewrite Module:** Enables clean, user-friendly URLs and proper request routing.  
- **PHP itself:** Required because osTicket’s backend logic (authentication, ticket handling, database queries) is written in PHP.  

**Instructions:**  
1. Download and install:  
   - [PHP Manager for IIS v1.5.0]  
   - [IIS URL Rewrite Module 2]  
2. Create a folder: `C:\PHP`
3. Download **PHP 7.3.8 (Non Thread Safe)** ZIP (x64 or x86 based on your system).
4. Unzip the **PHP 7.3.8 (Non Thread Safe)** folder into `C:\PHP`

<p>
<img width="494" height="404" alt="2  Install PHP Manager" src="https://github.com/user-attachments/assets/bd610767-74b8-41d3-85d2-f97cd1745259" />
</p>
<p>
<img width="490" height="383" alt="2  Install IIS Rewrite Module 2" src="https://github.com/user-attachments/assets/9a7619ac-157b-4739-9dfa-a047eb7285ec" />
</p>
<p>
<img width="1123" height="633" alt="2  Create PHP folder" src="https://github.com/user-attachments/assets/ea2b5f69-4961-4e3b-aeb2-fea60d1e3484" />  
</p>

---

<h3>STEP 3 – Install Microsoft Visual C++ Redistributable (x86, 2015-2022)</h3>

**Why**
The Visual C++ Redistributable provides runtime libraries required by PHP, MySQL, and osTIcket. Even on 64-bit systems. some components still run as 32-bit, making the **x86 version necessary** for compatibility.

**Instructions**
1. Download and install the **Visual C++ 2015-2022 Redistributable (x86)**

<p>
<img width="477" height="295" alt="3  Install Visual C++" src="https://github.com/user-attachments/assets/cde12d91-5819-4bc9-9c1d-d58baf3b43d2"/>
</p>

---

<h3>STEP 4 – Install MySQL 5.5.62</h3>

**Why**
osTicket requires a database. MySQL 5.5.62 is used here for **compatibility and stability** with osTicket.

**Instructions**
1. Run MySQL installer → choose **Typical Setup**.
2. Launch **MySQL Configuration Wizard**.
3. Select **Standard Configuration**.
4. Created a password for the **root** user (used later in HeidiSQL).
5. Finish installation

<p>
<img width="493" height="383" alt="4  Install MySQL" src="https://github.com/user-attachments/assets/beb94365-8711-4388-9f33-785205fd4df2"/>
</p>

---

<h3>STEP 5 – Configure PHP in IIS</h3>

**Why**
IIS needs to know which PHP version to use. This ensures osTicket's PHP scripts run properly.

**Instructions**
1. Open **IIS as Administrator**.
2. Go to **PHP Manager** → Register new PHP version.
3. Browse to the `C:\PHP` folder → select **php-cgi.exe**.
4. Reload IIS (**Stop → Start**) to apply changes.

<p>
<img width="1916" height="1039" alt="5  Register PHP" src="https://github.com/user-attachments/assets/2a85761f-f5d3-48fd-82f0-d250aec6f785"/>
</p>

---

<h3>STEP 6 – Install and Configure osTicket v1.15.8</h3>

**Why**
osTicket is the actual helpdesk/ticketing application. Installing it correctly ensures the system can run within IIS and communicate with PHP/MySQL.

**Instructions**
1. Download osTicket → copy the **upload** folder into `C:\inetpub\wwwroot`.
2. Rename the **upload** folder to **osTicket**.
3. Enable required PHP extentions in IIS Manager → PHP Manager:
  - `php_imap.dll`
  - `php_intl.dll`
  - `php_opcache.php`
4. Navigate to `C:\inetpub\wwwroot\osTicket\include` and rename `ost-sampleconfig.php` → `ost-config.php`.
5. Adjust file permissions:
  - Right-click `ost-congif.php` → Security → Advanced → disable inheritance → set proper permissions.
6. In IIS Manager, navigate to:
  - Connections → Sites → Default Web Site → osTicket.
  - On the right, click *Browse .80 (http)* to launch osTicket in a browser.

---

<h3>STEP 7 – Install and Configure HeidiSQL</h3>

**Why**
HeidiSQL is GUI tool client that makes it east to create/manage the osTicket MySQL database.

**Instructions**
1. Install **HeidiSQL 12.3.0.6589**.
2. Open HeidiSQL → click **New**.
3. Enter:
   - User: `root`
   - Password: (set during MySQL install)
4. Click **Open**.
5. Create a new database:
   - Right-click "Unnamed" → Create new → Database → name it **osTicket**.
6. Return to the osTicket setup in the browser:
   - Fill out **System Settings**.
   - Fill out **Admin User**.
   - Fill out **Database Settings**
     - Database Name: `osTicket`
     - MySQL Username: `root`
     - MySQL Password: (your MySQL root password)
    
<h1>🎉Congratulations!</h1>
You have succesfully installed and configured **osTicket** on Windows using **IIS, PHP, and MYSQL**.
