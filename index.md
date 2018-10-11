---
layout: default
---

# Admin: Install the server

Installing ownCloud on Linux from our Open Build Service packages is the preferred method (see Linux Package Manager Installation: https://doc.owncloud.org/server/latest/admin_manual/installation/linux_installation.html). These are maintained by ownCloud engineers, and you can use your package manager to keep your ownCloud server up-to-date.

## Prerequisites¶

The ownCloud tar archive contains all of the required third-party PHP libraries. As a result, no extra ones are, strictly, necessary. However, ownCloud does require that PHP has a set of extensions installed, enabled, and configured.

https://doc.owncloud.org/server/latest/admin_manual/installation/source_installation.html#prerequisites-label lists both the required and optional PHP extensions. If you need further information about a particular extension, please consult the relevant section of the extensions section of the PHP manual.

If you are using a Linux distribution, it should have packages for all the required extensions. You can check the presence of a module by typing php -m | grep -i <module_name>. If you get a result, the module is present.

## Install the Required Packages

### Centos 7

sudo yum install -y -q epel-release http://rpms.remirepo.net/enterprise/remi-release-7.rpm yum-utils \
&& sudo yum-config-manager --enable remi-php72 \
&& sudo yum update -y -q \
&& sudo yum install -y -q \
   httpd mariadb-server php72 php72-php php72-php-gd \
   php72-php-mbstring php72-php-mysqlnd php72-php-cli \
   php72-pecl-apcu redis php72-php-pecl-redis php72-php-common \
   php72-php-ldap mariadb-server mariadb \
&& sudo scl enable php72 bash

See https://doc.owncloud.org/server/latest/admin_manual/installation/source_installation.html#ubuntu-installation-label for package information used by other Linux distributions.

Now download the archive of the latest ownCloud version:

   -Go to the ownCloud Download Page at https://owncloud.org/install.

   -Go to Download ownCloud Server > Download > Archive file for server owners and download either the tar.bz2 or .zip archive.

   This downloads a file named owncloud-x.y.z.tar.bz2 or owncloud-x.y.z.zip (where x.y.z is the version number).

   -Download its corresponding checksum file, e.g. owncloud-x.y.z.tar.bz2.md5, or owncloud-x.y.z.tar.bz2.sha256.

   -Verify the MD5 or SHA256 sum:

    md5sum -c owncloud-x.y.z.tar.bz2.md5 < owncloud-x.y.z.tar.bz2
    sha256sum -c owncloud-x.y.z.tar.bz2.sha256 < owncloud-x.y.z.tar.bz2
    md5sum  -c owncloud-x.y.z.zip.md5 < owncloud-x.y.z.zip
    sha256sum  -c owncloud-x.y.z.zip.sha256 < owncloud-x.y.z.zip

   -You may also verify the PGP signature:

    wget https://download.owncloud.org/community/owncloud-x.y.z.tar.bz2.asc
    wget https://owncloud.org/owncloud.asc
    gpg --import owncloud.asc
    gpg --verify owncloud-x.y.z.tar.bz2.asc owncloud-x.y.z.tar.bz2

  Now you can extract the archive contents. Run the appropriate unpacking command for your archive type:

    tar -xjf owncloud-x.y.z.tar.bz2
    unzip owncloud-x.y.z.zip

   This unpacks to a single owncloud directory. Copy the ownCloud directory to its final destination. When you are running the Apache HTTP server, you may safely install ownCloud in your Apache document root:

    cp -r owncloud /path/to/webserver/document-root

   where /path/to/webserver/document-root is replaced by the document root of your Web server:

    cp -r owncloud /var/www

On other HTTP servers, it is recommended to install ownCloud outside of the document root.

## Configure Apache Web Server

https://doc.owncloud.org/server/latest/admin_manual/installation/source_installation.html#apache-configuration-label

## Enable SSL

https://doc.owncloud.org/server/latest/admin_manual/installation/source_installation.html#enabling-ssl-label

## Run the Installation Wizard

After restarting Apache, you must complete your installation by running either the Graphical Installation Wizard or on the command line with the occ command. To enable this, temporarily change the ownership on your ownCloud directories to your HTTP user (see Set Strong Directory Permissions (https://doc.owncloud.org/server/latest/admin_manual/installation/source_installation.html#strong-perms-label) to learn how to find your HTTP user):

    chown -R www-data:www-data /var/www/owncloud/

-To use occ see Command Line Installation (https://doc.owncloud.org/server/latest/admin_manual/installation/command_line_installation.html).

-To use the graphical Installation Wizard see The Installation Wizard (https://doc.owncloud.org/server/latest/admin_manual/installation/installation_wizard.html).


https://doc.owncloud.org/server/latest/admin_manual/installation/source_installation.html#installation-wizard-label

## Set Strong Directory Permissions

After completing the installation, you must immediately set the directory permissions in your ownCloud installation as strictly as possible for stronger security. After you do so, your ownCloud server will be ready to use.

https://doc.owncloud.org/server/latest/admin_manual/installation/source_installation.html#strong-perms-label

## Set Trusted Domains  

All URLs used to access your ownCloud server must be whitelisted in your config.php file, under the trusted_domains setting. Users are allowed to log into ownCloud only when they point their browsers to a URL that is listed in the trusted_domains setting.

A typical configuration looks like this:

    'trusted_domains' => [
       0 => 'localhost',
       1 => 'server1.example.com',
       2 => '192.168.1.50',
    ],

The loopback address, 127.0.0.1, is automatically whitelisted, so as long as you have access to the physical server you can always log in. In the event that a load-balancer is in place, there will be no issues as long as it sends the correct X-Forwarded-Host header.

For further information on improving the quality of your ownCloud installation, please see the Configuration Notes & Tips guide (https://doc.owncloud.org/server/latest/admin_manual/installation/configuration_notes_and_tips.html).

## Install from the Command Line

ownCloud can be installed entirely from the command line. This is convenient for scripted operations and for systems administrators who prefer using the command line over a GUI. It involves five steps:

   -Ensure your server meets the ownCloud prerequisites
   -Download and unpack the source
   -Install using the occ command
   -Set the correct owner and permissions
   -Optional post#.installation considerations

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


## Run the Installation Wizard

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





Text can be **bold**, _italic_, or ~~strikethrough~~.

[Link to another page](./another-page.html).

There should be whitespace between paragraphs.

There should be whitespace between paragraphs. We recommend including a README, or a file with information about your project.

# Header 1

This is a normal paragraph following a header. GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.

## Header 2

> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://assets-cdn.github.com/images/icons/emoji/octocat.png)

### Large image

![Branching](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
