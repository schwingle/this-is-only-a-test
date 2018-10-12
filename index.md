---
layout: default
---

# Install the ownCloud Server on a Linux Distribution

## Install with a Package Manager
On a Linux distribution, you can use a package manager to install the server (https://doc.owncloud.org/server/latest/admin_manual/installation/linux_installation.html). The recommended package to use is owncloud-files. It only installs ownCloud, and does not install Apache, a database, or any of the required PHP dependencies. Package managers should only be used for single-server setups.

1. Install your own LAMP stack, as doing so allows you to create your own custom LAMP stack without dependency conflicts with the ownCloud package. Configurations are available for the following Linux distributions:

    -Ubuntu 14.04 & 16.04
    -Debian 7 & 8
    -RHEL 6 & 7
    -CentOS 7.2 & 7.3
    -SLES 11SP4 & 12SP2
    -openSUSE Leap 42.2 & 42.3

2. Update the package manager configuration.
3. Set strong directory permissions.
4. Run the Installation Wizard.

### Installation Prerequisites

Your Linux distribution should have packages for all the required extensions. You can check the presence of a module by typing 

            php -m | grep -i <module_name>
            
If you get a result, the module is present.

### Update the Package Manager Configuration

- Go to http://download.owncloud.org/download/repositories/10.0/owncloud/.
- Select your Linux distribution.
- Follow the instructions to add the repository and install manually.

### Set Strong Directory Permissions

Set the permissions on your ownCloud directories as strictly as possible, and for proper server operations. This should be done immediately after the initial installation and before running the setup.

Your HTTP user must own the config/, data/, apps/ respectively the apps-external/ directories so that you can configure ownCloud, create, modify and delete your data files, and install apps via the ownCloud Web interface.

You can find your HTTP user in your HTTP server configuration files, or you can use PHP Version and Information (Look for the User/Group line).

   -The HTTP user and group in Debian/Ubuntu is www-data.
   -The HTTP user and group in Fedora/CentOS is apache.
   -The HTTP user and group in Arch Linux is http.
   -The HTTP user in openSUSE is wwwrun, and the HTTP group is www.

### Run the Installation Wizard

When the ownCloud prerequisites are fulfilled and all ownCloud files are installed, the last step to completing the installation is running the Installation Wizard.

Important! It is strongly recommended that you protect the installer through some form of password authentication, or access control. If the installer is left unprotected when exposed to the public internet, there is the possibility that a malicious actor could finish the installation and block you out — or worse. So please ensure that only you — or someone from your organization — can access the web installer.

Run the Installation Wizard to complete the installation. 

   1. Point your web browser to http://localhost/owncloud.
   2. Enter your desired administrator’s username and password.
   3. Set your data directory location and enter your database credentials.
   4. Click “Finish Setup”.

  You can now start using your new ownCloud server.

For more information, see https://doc.owncloud.org/server/latest/admin_manual/installation/installation_wizard.html.


# Configure the Server

After installing your ownCloud server, click “Storage and Database” in the Installation Wizard to expose additional installation configuration options for your ownCloud data directory and database.

There are many additional server configuration options you can choose. See https://doc.owncloud.org/server/latest/admin_manual/configuration/server/ for more information.

Besides configuration of the server itself, https://doc.owncloud.org/server/latest/admin_manual/configuration/ page contains information about configuring other ownCloud features:

   -Database Configuration
   -File Sharing and Management
   -How To Install and Configure an LDAP Proxy-Cache Server
   -Mimetypes Management
   -User Management


## Set the Data Directory

You should locate your ownCloud data directory outside of your Web root if you are using an HTTP server other than Apache, or you may wish to store your ownCloud data in a different location for other reasons (e.g. on a storage server).

Your ownCloud data directory must be exclusive to ownCloud and not be modified manually by any other process or user.

It is best to configure your data directory location at installation, as it is difficult to move after installation. You may put it anywhere; in this example is it located in /var/oc_data. This directory must already exist, and must be owned by your HTTP user (see Set Strong Directory Permissions).

https://doc.owncloud.org/server/latest/admin_manual/_images/install-wizard-a1.png

## Configure the Database

When installing ownCloud Server & ownCloud Enterprise editions the administrator may choose one of 4 supported database products. These are:

   -SQLite
   -MYSQL/MariaDB
   -PostgreSQL
   -Oracle 11g (Enterprise-edition only)

Select your database and enter connection credentials.

# Configure User Access

On your server's Admin page, specify the host server name, for example localhost, hostname, hostname.example.com, or the IP address. To specify a port use hostname:####; to specify a Unix socket use localhost:/path/to/socket.

This information is stored in 'dbhost' in the  config/config.php file.

    'dbhost' => '',

ownCloud uses the config/config.php file to control server operations. Most options are configurable on your Admin page, so it is usually not necessary to edit config/config.php.

(https://doc.owncloud.org/server/10.0/admin_manual/configuration/server/config_sample_php_parameters.html)

# Add a User Account

Create new users on the User management page of your ownCloud Web UI.

## Create a New User Account

   1. Enter the new user’s __Login Name__ and their initial __Password__.
   2. Optionally, assign __Groups__ memberships
   3. Click the __Create__ button

![CreateUser]https://doc.owncloud.org/server/latest/admin_manual/_images/users-create.png

Login names may contain letters (a-z, A-Z), numbers (0-9), dashes (-), underscores (\_), periods (.) and at signs (@). After creating the user, you may fill in their Full Name if it is different than the login name, or leave it for the user to complete.

If you have checked __Send email to new user__ in the control panel on the lower left sidebar, you may also enter the new user’s email address, and ownCloud will automatically send them a notification with their new login information. You may edit this email using the email template editor on your Admin page (see Email Configuration: https://doc.owncloud.org/server/10.0/admin_manual/configuration/server/email_configuration.html).

https://doc.owncloud.org/server/10.0/admin_manual/configuration/user/user_configuration.html#creating-a-new-user

# Connect to the Server

Connect to the server using the a browser, supplying the server's IP address and port 8080.

## Connect with a Desktop Client

The ownCloud Desktop Client (https://doc.owncloud.org/desktop/latest/navigating.html) remains in the background and is visible as an icon in the system tray (Windows, KDE), menu bar (macOS), or notification area (Linux). A right-click on the systray icon opens a menu for quick access to your accounts.

[!Systray]https://doc.owncloud.org/desktop/latest/_images/menu.png

## Connect with a Mobile Client

Open a Web browser and point it to your ownCloud server. Log in and look on your Personal page for a link to the ownCloud app on iTunes (iOS: https://doc.owncloud.org/ios/) or Google Play (Android: https://doc.owncloud.org/android/android_app.html). When you install the ownCloud app and open it you’ll be prompted for your ownCloud server URL and login. When it connects it opens to your Files page.

