---
layout: default
---

# Admin: Install the ownCloud server

ownCloud server runs on Linux. On a Linux distribution, you can use a package manager to install the server (https://doc.owncloud.org/server/latest/admin_manual/installation/linux_installation.html). The recommended package to use is owncloud-files. It only installs ownCloud, and does not install Apache, a database, or any of the required PHP dependencies. Package managers should only be used for single-server setups. For production environments, we recommend installing from the tar archive.

If there are no packages for your Linux distribution, or you prefer installing from the source tarball, you can set up ownCloud from scratch using a classic LAMP stack (Linux, Apache, MySQL/MariaDB, PHP). 

Installing ownCloud from Open Build Service packages is the preferred method. These are maintained by ownCloud engineers, and you can use your package manager to keep your ownCloud server up-to-date. https://doc.owncloud.org/server/latest/admin_manual/installation/source_installation.html. 

https://download.owncloud.org/download/repositories/stable/owncloud/index.html


## Prerequisites¶

If you are using a Linux distribution, it should have packages for all the required extensions. You can check the presence of a module by typing 

            php -m | grep -i <module_name>
            
If you get a result, the module is present.

The ownCloud tar archive contains all of the required third-party PHP libraries. As a result, no extra ones are, strictly, necessary. However, ownCloud does require that PHP has a set of extensions installed, enabled, and configured.

https://doc.owncloud.org/server/latest/admin_manual/installation/source_installation.html#prerequisites-label lists both the required and optional PHP extensions. If you need further information about a particular extension, please consult the relevant section of the extensions section of the PHP manual.


### Update Package Manager

- Go to http://download.owncloud.org/download/repositories/10.0/owncloud/
- Select your Linux distribution
- Follow the instructions to add the repository and install manually

Once your package manager has been updated, follow the rest of the instructions on the download page to install ownCloud. Once ownCloud is installed, run the Installation Wizard (https://doc.owncloud.org/server/latest/admin_manual/installation/installation_wizard.html) to complete your installation.

https://doc.owncloud.org/server/latest/admin_manual/installation/source_installation.html#installation-wizard-label


## Install ownCloud from the Command Line

ownCloud can be installed entirely from the command line. This is convenient for scripted operations and for systems administrators who prefer using the command line over a GUI. It involves five steps:

   1. Ensure your server meets the ownCloud prerequisites
   2. Download and unpack the source
   3. Install using the occ command
   4. Set the correct owner and permissions
   5. Optional post-installation considerations

   To install ownCloud, first download the source (whether community or enterprise) directly from ownCloud, and then unpack (decompress) the tarball into the appropriate directory.

   With that done, you next need to set your webserver user to be the owner of your unpacked owncloud directory, as in the example below.

    $ sudo chown -R www-data:www-data /var/www/owncloud/

   With those steps completed, next use the occ command, from the root directory of the ownCloud source, to perform the installation. This removes the need to run the Graphical Installation Wizard. Here’s an example of how to do it:

    # Assuming you’ve unpacked the source to /var/www/owncloud/
    $ cd /var/www/owncloud/
    $ sudo -u www-data php occ maintenance:install \
       --database "mysql" --database-name "owncloud" \
       --database-user "root" --database-pass "password" \
       --admin-user "admin" --admin-pass "password"

Important! You must run occ as your HTTP user. See Run occ As Your HTTP User (https://doc.owncloud.org/server/latest/admin_manual/configuration/server/occ_command.html#http-user-label)

If you want to use a directory other than the default (which is data inside the root ownCloud directory), you can also supply the --data-dir switch. For example, if you were using the command above and you wanted the data directory to be /opt/owncloud/data, then add --data-dir /opt/owncloud/data to the command.

When the command completes, apply the correct permissions to your ownCloud files and directories (see Set Strong Directory Permissions: https://doc.owncloud.org/server/latest/admin_manual/installation/source_installation.html#strong-perms-label). This is extremely important, as it helps protect your ownCloud installation and ensure that it will operate correctly. See Command Line Installation (https://doc.owncloud.org/server/latest/admin_manual/configuration/server/occ_command.html#command-line-installation-label) for more information.

### Set Strong Directory Permissions

After completing the installation, you must immediately set the directory permissions in your ownCloud installation as strictly as possible for stronger security. After you do so, your ownCloud server will be ready to use.

https://doc.owncloud.org/server/latest/admin_manual/installation/source_installation.html#strong-perms-label


### Run the Installation Wizard

If you prefer to install ownCloud using a GUI, follow these instructions.

When the ownCloud prerequisites are fulfilled and all ownCloud files are installed, the last step to completing the installation is running the Installation Wizard. This involves just three steps:

   1. Point your web browser to http://localhost/owncloud
   2. Enter your desired administrator’s username and password.
   3. Click “Finish Setup”.

  You’re now finished and can start using your new ownCloud server.

https://doc.owncloud.org/server/latest/admin_manual/installation/installation_wizard.html



# Admin: Configure the server

After installing your ownCloud server, click “Storage and Database” in the Installation Wizard to expose additional installation configuration options for your ownCloud data directory and database.

# Admin: Configure user access
(using the server's IP address and port 8080)

On your Admin page, specify the host server name, for example localhost, hostname, hostname.example.com, or the IP address. To specify a port use hostname:####; to specify a Unix socket use localhost:/path/to/socket.

This information is stored in 'dbhost' in the  config/config.php file.

    'dbhost' => '',

ownCloud uses the config/config.php file to control server operations. Most options are configurable on your Admin page, so it is usually not necessary to edit config/config.php.

(https://doc.owncloud.org/server/10.0/admin_manual/configuration/server/config_sample_php_parameters.html)

# Admin: Add a user account

Create new users on the User management page of your ownCloud Web UI.

## Creating a New User

To create a user account:

   -Enter the new user’s Login Name and their initial Password
   -Optionally, assign Groups memberships
   -Click the Create button

![CreateUser]https://doc.owncloud.org/server/latest/admin_manual/_images/users-create.png

Login names may contain letters (a-z, A-Z), numbers (0-9), dashes (-), underscores (\_), periods (.) and at signs (@). After creating the user, you may fill in their Full Name if it is different than the login name, or leave it for the user to complete.

If you have checked Send email to new user in the control panel on the lower left sidebar, you may also enter the new user’s email address, and ownCloud will automatically send them a notification with their new login information. You may edit this email using the email template editor on your Admin page (see Email Configuration: https://doc.owncloud.org/server/10.0/admin_manual/configuration/server/email_configuration.html).

https://doc.owncloud.org/server/10.0/admin_manual/configuration/user/user_configuration.html#creating-a-new-user

# User: Connect to the server with a desktop or mobile client

## User: Connect with a desktop client

The ownCloud Desktop Client (https://doc.owncloud.org/desktop/latest/navigating.html) remains in the background and is visible as an icon in the system tray (Windows, KDE), menu bar (macOS), or notification area (Linux). A right-click on the systray icon opens a menu for quick access to your accounts.

[!Systray]https://doc.owncloud.org/desktop/latest/_images/menu.png

## User: Connect with a mobile client

Open a Web browser and point it to your ownCloud server. Log in and look on your Personal page for a link to the ownCloud app on iTunes (iOS: https://doc.owncloud.org/ios/) or Google Play (Android: https://doc.owncloud.org/android/android_app.html). When you install the ownCloud app and open it you’ll be prompted for your ownCloud server URL and login. When it connects it opens to your Files page.

