# osticket-prereqs
<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Prerequisites and Installation</h1>

This guide walks you through installing and configuring **osTicket v1.15.8** on Windows using **IIS, PHP and MySQL**. Each step includes a brief explanation of why it is important.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> (22H2)

## Table of Contents
- [STEP 1 — Enable IIS with CGI](#step-1--enable-iis-with-cgi)
- [STEP 2 — Install PHP Manager and IIS Rewrite Modules](#step-2--install-php-manager-and-iis-rewrite-modules)
- [STEP 3 — Install Microsoft Visual C++ Redistributable (x86, 2015-2022)](#step-3--install-microsoft-visual-c-redistributable-x86-2015-2022)
- [STEP 4 — Install MySQL 5.5.62](#step-4--install-mysql-5562)
- [STEP 5 — Configure PHP in IIS](#step-5--configure-php-in-iis)
- [STEP 6 — Install and Configure osTicket v1.15.8](#step-6--install-and-configure-osticket-v1158)
- [STEP 7 — Install and Configure HeidiSQL](#step-7--install-and-configure-heidisql)

<h2>Installation Steps</h2>

### STEP 1 – Enable IIS with CGI

**Why:**  
IIS (Internet Information Services) is Microsoft’s web server platform. Enabling CGI (Common Gateway Interface) allows IIS to run certain server-side scripts that osTicket requires. Without IIS, your system cannot host the osTicket web application.  

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

### STEP 2 – Install PHP Manager and IIS Rewrite Modules

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

### STEP 3 – Install Microsoft Visual C++ Redistributable (x86, 2015-2022)

**Why:**  
The Visual C++ Redistributable provides runtime libraries required by PHP, MySQL, and osTIcket. Even on 64-bit systems. some components still run as 32-bit, making the **x86 version necessary** for compatibility.

**Instructions:**  
1. Download and install the **Visual C++ 2015-2022 Redistributable (x86)**

<p>
<img width="477" height="295" alt="3  Install Visual C++" src="https://github.com/user-attachments/assets/cde12d91-5819-4bc9-9c1d-d58baf3b43d2"/>
</p>

---

### STEP 4 – Install MySQL 5.5.62

**Why:**  
osTicket requires a database. MySQL 5.5.62 is used here for **compatibility and stability** with osTicket.

**Instructions:**  
1. Run MySQL installer → choose **Typical Setup**.
2. Launch **MySQL Configuration Wizard**.
3. Select **Standard Configuration**.
4. Created a password for the **root** user (used later in HeidiSQL).
5. Finish installation.

<p>
<img width="493" height="383" alt="4  Install MySQL" src="https://github.com/user-attachments/assets/beb94365-8711-4388-9f33-785205fd4df2"/>
</p>

---

### STEP 5 – Configure PHP in IIS

**Why:**  
IIS needs to know which PHP version to use. This ensures osTicket's PHP scripts run properly.

**Instructions:**  
1. Open **IIS as Administrator**.
2. Go to **PHP Manager** → Register new PHP version.
3. Browse to the `C:\PHP` folder → select **php-cgi.exe**.
4. Reload IIS (**Stop → Start**) to apply changes.

<p>
<img width="1916" height="1039" alt="5  Register PHP" src="https://github.com/user-attachments/assets/2a85761f-f5d3-48fd-82f0-d250aec6f785"/>
</p>

---

### STEP 6 – Install and Configure osTicket v1.15.8

**Why:**  
osTicket is the actual helpdesk/ticketing application. Installing it correctly ensures the system can run within IIS and communicate with PHP/MySQL.

**Instructions:**  
1. Download osTicket v1.15.8 → copy the **upload** folder into `C:\inetpub\wwwroot`.
2. Rename the **upload** folder to **osTicket**.
3. Reload IIS (**Stop → Start**) to apply changes.
4. Enable required PHP extentions in IIS Manager → PHP Manager:
  - `php_imap.dll`
  - `php_intl.dll`
  - `php_opcache.php`
4. Navigate to `C:\inetpub\wwwroot\osTicket\include` and rename `ost-sampleconfig.php` → `ost-config.php`.
5. Adjust file permissions:
  - Right-click `ost-congif.php` → Security → Advanced → disable inheritance → set proper permissions.
6. In IIS Manager, navigate to:
  - Connections → Sites → Default Web Site → osTicket.
  - On the right, click *Browse .80 (http)* to launch osTicket in a browser.

<p>
<img width="1421" height="748" alt="6  Enable osTicket extensions1 5" src="https://github.com/user-attachments/assets/22d9e8d3-42b7-41d5-9a5b-debd7050f7ab"/>
</p>
<p>
<img width="1560" height="823" alt="6  ost-config" src="https://github.com/user-attachments/assets/c45a4f47-9489-49c3-a6be-48b1ad2d1eca"/>
</p>
<p>
<img width="1422" height="747" alt="6  Load osTicket site1" src="https://github.com/user-attachments/assets/6e996197-41f8-4b82-a4ee-30d4568582d7"/>
</p>

---

### STEP 7 – Install and Configure HeidiSQL

**Why**  
HeidiSQL is GUI tool client that makes it east to create/manage the osTicket MySQL database.

**Instructions:**  
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

<p>
<img width="593" height="460" alt="7  Instal HeidiSQL" src="https://github.com/user-attachments/assets/6c235264-d250-4fd0-8960-98e5654914e1"/>
</p>
<p>
<img width="681" height="479" alt="7  heidi" src="https://github.com/user-attachments/assets/30d9c176-91fd-4c46-82b6-ff604c865147"/>
</p>
<p>
<img width="932" height="590" alt="7  heidi2" src="https://github.com/user-attachments/assets/a8b9c546-df0b-4e55-adb0-365323d469c6"/>
</p>
<p>
<img width="946" height="968" alt="7  Screenshot 2025-09-02 011732" src="https://github.com/user-attachments/assets/fab9b2e6-5b1e-489e-838f-44072b91fcd5"/>
</p>
<p>
<img width="743" height="378" alt="7  Screenshot 2025-09-02 222217" src="https://github.com/user-attachments/assets/699057e8-29e9-4670-ad43-2799f3cca149"/>
</p>

---

<h3>🎉Congratulations!</h3>

You have succesfully installed and configured **osTicket** on Windows using **IIS, PHP, and MYSQL**.
