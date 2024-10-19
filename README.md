# Day-24-Setting-Up-and-Configuring-osTicket

In this challenge, we will walk you through the process of setting up and configuring osTicket, a powerful open-source ticketing system. By the end of this guide, you’ll have a fully operational osTicket instance ready for your team to start managing requests and incidents. Let’s dive in!

## Step 1: Deploy a Server on Vultr

To start, head over to Vultr and log in to your account. Follow these steps to deploy your server:

### Deploy a New Server:
1. Click on the **Deploy** button in the top-right corner.
2. Select **Deploy New Server**.
3. Since osTicket doesn’t require heavy resources, choose **Cloud Compute (Shared CPU)**.

### Configure the Server:
- **Location:** Choose your preferred location (I selected London).
- **Operating System:** Select **Windows Standard 2022**.
- **Plan:** Use the default plan, which includes 55 GB SSD, 1 vCPU, 2 GB RAM, and 55 GB storage.
- **Extras:** Unselect Auto Backup and IPv6, but enable Virtual Private Cloud 2.0.
- **Name Your Server:** I named mine **Phoenix-osTicket**, but you can choose your own.
- **Deploy:** Click **Deploy Server** and wait for your server to start running.

Once the server is up, you’ll need to Remote Desktop Protocol (RDP) into the machine. Copy the username and password provided, and log in to your new server.

## Step 2: Set Up a Firewall

Before we proceed, it’s essential to secure your server since it will host a web server that could be accessible to the public. Here’s how to add a firewall:

### Go to Firewall Settings:
1. Navigate to **Settings** and then select **Firewall**.
2. I used the **Phoenix-SOC-Challenge** firewall.

**Why Set Up a Firewall?** The firewall ensures that only authorized users can access your ticketing system, preventing unauthorized traffic from accessing the web server.

## Step 3: Install XAMPP to Host the Web Server

We need a web server to host osTicket. For this, we’ll use XAMPP, which is an easy-to-use package from Apache.

### Download XAMPP:
1. Search for XAMPP and download it from Apache Friends.
2. Choose version **8.2.12** and click **Download**.

### Install XAMPP:
1. Run the installer and keep all settings as default. Click **Next** until the installation is complete.
2. Once installed, open the **XAMPP Control Panel**.

## Step 4: Configure XAMPP for osTicket

With XAMPP installed, we now need to configure it for osTicket.

### Update Apache Configuration:
1. Navigate to `C:\XAMPP` and find the Apache configuration file.
2. Change the domain name from `localhost` to your server’s public IP address.
3. Save the changes.

### Configure phpMyAdmin:
1. In the XAMPP directory, go to `phpMyAdmin/config.inc.php`.
2. Make a backup of this file and open the original with Notepad.
3. Change the server host from `127.0.0.1` to your osTicket server's public IP address.
4. Save and exit.

### Set Up Firewall Rules:
1. Open **Windows Defender Firewall** and navigate to **Inbound Rules**.
2. Create a new rule to allow inbound connections on **Port 80** and **Port 443**.
3. Name this rule and save it.

### Start Apache and MySQL Services:
1. Open the **XAMPP Control Panel** and start Apache and MySQL services.
2. Click on **Admin** under Apache to access phpMyAdmin.

## Step 5: Install osTicket

Now that your web server is ready, it’s time to install osTicket.

### Download osTicket:
1. Visit osTicket and download the **Self-Hosted version (1.18.1)**.
2. Extract the downloaded file.

### Move osTicket to XAMPP:
1. In your XAMPP directory, navigate to `htdocs`.
2. Create a new folder called **osTicket** and move the extracted osTicket files into it.

### Launch the osTicket Setup:
1. Open your browser and go to `http://[Your Server IP]/osTicket/upload`.
2. Follow the setup instructions.

### Fix Missing Extensions:
During the setup, you may see warnings about missing extensions like GDlib or PHP Imap. For this challenge, these extensions aren’t necessary, so you can safely ignore the warnings and proceed with the installation.

### Configure osTicket:
- Set up your Help Desk Name (e.g., **Phoenix Support**).
- Fill in your default email, admin name, and other relevant details.

## Step 6: Create and Configure the Database

Before completing the osTicket installation, we need to create a MySQL database:

### Create the Database in phpMyAdmin:
1. Go to phpMyAdmin, click **New**, and create a database named **Phoenix-30-Day-DB**.

### Grant Privileges:
1. Navigate to **User Accounts** and select the **root user**.
2. Ensure that this user has privileges to access the new database.

### Complete osTicket Installation:
1. Return to the osTicket setup page and fill in the database details (DB name, root username, and password).
2. Click **Install** and wait for the process to complete.

## Step 7: Final Configurations

Once osTicket is installed, you’ll need to finalize a few settings:

### Set File Permissions:
1. Open **PowerShell** as an administrator.
2. Navigate to the osTicket configuration file (`ost-config.php`), located in `htdocs/include`.
3. Run the following command to reset permissions:
   ```bash
   icacls .\ost-config.php /reset

You should see a success message.

### Access osTicket Admin Panel:
1. Copy the **Staff Control Panel** URL and open it in your browser.
2. Log in using your credentials.

## Step 8: Outsider Login (End-User Access)

Once osTicket is set up, end-users (customers or internal team members) will be able to submit tickets through the client portal. Here’s how they can access it:

### Access the Client Portal:
Users can visit the public osTicket URL. This is typically in the format:

```bash
http://[Your Server IP]/osTicket/upload
```
This is the page where outsiders (or customers) can log in or create new tickets.

### Submit a Ticket:
Users can create new tickets by providing their name, email, and issue details.

## Step 9: Admin Login (Staff and Agent Access)

The admin login is where your support team will manage and resolve incoming tickets. Here’s how to log in as an admin:

### Access the Admin Panel:
To access the admin (staff) control panel, visit the following URL:

```bash
http://[Your Server IP]/osTicket/upload/scp
```
This will prompt you to log in with the credentials you set during the installation process.

### Admin Dashboard:
After logging in, admins will have access to the full osTicket dashboard where they can:
- View, assign, and respond to tickets.
- Manage staff and departments.
- Configure system settings and plugins.
- Access various reports and analytics about ticket activity.

## Conclusion

Congratulations! You’ve successfully set up your own osTicket instance. You now have a fully functional ticketing system ready to handle requests. In my next blog, I’ll cover how to integrate osTicket with your tech stack to automate alert generation and ticket creation.

