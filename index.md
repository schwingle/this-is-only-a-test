---
layout: default
---
# Install the ownCloud Server
* * *
On a Linux distribution, you can use a package manager to install the server.

- Go to [http://download.owncloud.org/download/repositories/10.0/owncloud/](http://download.owncloud.org/download/repositories/10.0/owncloud/).
- Select your Linux distribution.
- Follow the instructions to add the repository and install manually.

Next, run the Installation Wizard.

## Run the Installation Wizard

Run the Installation Wizard to complete the installation.

  1. Point your web browser to [http://localhost/owncloud](http://localhost/owncloud).
  2. Enter your desired administrator’s username and password.
  3. Click __Storage and Database__ to set your data directory location and database connection.
  
      - Data directory: Your ownCloud data directory must already exist, be owned by your HTTP user, and be exclusive to ownCloud (not be modified manually by any other process or user).
      - Database: You may choose one of 4 supported database product: SQLite, MYSQL/MariaDB, PostgreSQL, and Oracle 11g (Enterprise-edition only).
  4. Click __Finish Setup__.

# Configure the ownCloud Server
* * *
During setup in the Installation Wizard, click __Storage and Database__ to set your data directory and database connection.

There are many additional server configuration options you can choose. See [Server Configuration](https://doc.owncloud.org/server/latest/admin_manual/configuration/server/) for more information.

## Configure User Access

On your server's Admin page, specify the host server name or the IP address.
  - To specify a port, use hostname:####.
  - To specify a Unix socket, use localhost:/path/to/socket.

This information is stored in the 'dbhost' parameter in the  config/config.php file.

    'dbhost' => '',

Learn more about config.php parameters at [Core Config.php Parameters](https://doc.owncloud.org/server/latest/admin_manual/configuration/server/config_sample_php_parameters.html).

### Create a New User Account

Create new users on the User management page of your ownCloud Web UI.

   1. Enter the new user’s login name and initial password.
   2. Optionally, assign groups.
   3. Click __Create__.

# Connect to your ownCloud
* * *
## Connect with a Browser

Connect to the server with a browser using the server's IP address or host server name. Specify a port if required by the administrator. Then enter your login credentials.

## Connect with a Desktop Client

The ownCloud Desktop Client remains in the background and is visible as an icon in the system tray (Windows, KDE), menu bar (macOS), or notification area (Linux). Right-click on the icon to open a context menu and access your accounts.

## Connect with a Mobile Client

Install the ownCloud app on a mobile device and open it. Enter your ownCloud server URL and login credentials.

* * *
All source information is from the [ownCloud 10.0.10 Server Administration Manual](https://doc.owncloud.org/server/latest/admin_manual/contents.html)
