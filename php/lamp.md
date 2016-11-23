# How To Install the LAMP stack on Ubuntu 14.04

## LAMP & Wordpress

### Install Apache

```bash
sudo apt-get update
sudo apt-get install apache2
```

### Install MySQL

```bash
sudo apt-get install mysql-server php5-mysql
```

tell MySQL to create its database directory structure where it will store its information.
```bash
sudo mysql_install_db
```
run a simple security script that will remove some dangerous defaults and lock down access to our database system
```bash
sudo mysql_secure_installation
```

### Install PHP

```bash
sudo apt-get install php5 libapache2-mod-php5 php5-mcrypt
```

Update apache config file to look for index.php first:

```bash
sudo vim /etc/apache2/mods-enabled/dir.conf

<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

Restart apache:
```bash
sudo service apache2 restart
```

### Test PHP Installation


```bash
sudo vim /var/www/html/info.php
```

Put inside the file:
```php
<?php
phpinfo();
?>
```

Run on the browser:

```
http:/localhost/info.php
```

Source: https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-14-04


## Wordpress - How To Install Wordpress on Ubuntu 14.04

### 1 - Create a MySQL Database and User for WordPress

Log into the MySQL root:

```bash
mysql -u root -p
```

Create the database:

```bash
CREATE DATABASE database_name;
```

 create a separate MySQL user account that we will use exclusively to operate on our new database.
 Creating one-function databases and accounts is a good idea from a management and security standpoint.

 ```bash
 CREATE USER wordpressuser@localhost IDENTIFIED BY 'password';
 ```

Grant our user account access to our database:

```bash
GRANT ALL PRIVILEGES ON wordpress.* TO wordpressuser@localhost;
```

flush the privileges so that the current instance of MySQL knows about the recent privilege changes we've made:

```bash
FLUSH PRIVILEGES;
```

Get out of MySQL:

```bash
exit
```

### 2 - Download WordPress

get the latest version:

```bash
wget http://wordpress.org/latest.tar.gz
```

extract the files to rebuild the WordPress directory:

```bash
tar xzvf latest.tar.gz
```

Install the necessary plugins that will allow you to work with images and will also allow you to install plugins and update portions of your site using your SSH login credentials:

```bash
sudo apt-get update
sudo apt-get install php5-gd libssh2-php
```

### 3 - Configure WordPress

Move into the WordPress directory that you just unpacked:

````bash
cd wordpress/
```

A sample configuration file that mostly matches the configuration we need is included by default. However, we need to copy it to the default configuration file location to get WordPress to recognize the file. Type:

```bash
cp wp-config-sample.php wp-config.php
```

Open the config file:

```bash
vim wp-config.php
```

this file is almost entirely suitable for our needs already. The only modifications we need to make are to the parameters that hold our database information.

We will need to find the settings for `DB_NAME`, `DB_USER`, and `DB_PASSWORD` in order for WordPress to correctly connect and authenticate to the database we created.

Fill in the values of these parameters with the information for the database you created. It should look like this:

```php
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', 'wordpressuser');

/** MySQL database password */
define('DB_PASSWORD', 'password');
```

When you are finished, save and close the file.

### 4 - Copy Files to the Document Root

Now that we have our application configured, we need to copy it into Apache's document root, where it can be served to visitors of our website.

One of the easiest and most reliable way of transferring files from directory to directory is with the rsync command. This preserves permissions and has good data integrity features.

The location of the document root in the Ubuntu 14.04 LAMP guide is /var/www/html/. We can transfer our WordPress files there by typing:

```bash
sudo rsync -avP ~/wordpress/ /var/www/html/
```

This will safely copy all of the contents from the directory you unpacked to the document root.

We should now move into the document root to make some final permissions changes

```bash
cd /var/www/html
```

You will need to change the ownership of our files for increased security.

We want to give user ownership to the regular, non-root user (with sudo privileges) that you plan on using to interact with your site. This can be your regular user if you wish, but some may suggest that you create an additional user for this process. It is up to you which you choose.

The group ownership we will give to our web server process, which is www-data. This will allow Apache to interact with the content as necessary.

We can quickly assign these ownership values by typing:

```bash
sudo chown -R user:www-data *
```

While we are dealing with ownership and permissions, we should also look into assigning correct ownership on our uploads directory. This will allow us to upload images and other content to our site. Currently, the permissions are too restrictive.

First, let's manually create the uploads directory beneath the wp-content directory at our document root. This will be the parent directory of our content:

```bash
mkdir /var/www/html/wp-content/uploads
```

We have a directory now to house uploaded files, however the permissions are still too restrictive. We need to allow the web server itself to write to this directory. We can do this by assigning group ownership of this directory to our web server, like this:

```bash
sudo chown -R :www-data /var/www/html/wp-content/uploads
```

This will allow the web server to create files and directories under this directory, which will permit us to upload content to the server.

### 5 - Complete Installation through the Web Interface

Now that you have your files in place and your software is configured, you can complete the installation through the web interface.

In your web browser, navigate to your server's domain name or public IP address:

```
http://localhost
```

Fill out the information for the site and the administrative account you wish to make. When you are finished, click on the install button at the bottom.

After that, log into with the acccount you just created.

## Step Six (Optional) - Configure Pretty Permalinks for WordPress

By default, WordPress creates URLs dynamically that look something like this:

```
http://server_domain_name_or_IP/?p=1
```

There are a few things we need to do to get this to work with Apache on Ubuntu 14.04.

#### Modifying Apache to Allow URL Rewrites

First, we need to modify the Apache virtual host file for WordPress to allow for .htaccess overrides. You can do this by editing the virtual host file.

By default, this is 000-default.conf, but your file might be different if you created another configuration file:

```bash
sudo vim /etc/apache2/sites-available/000-default.conf
```

We should set the ServerName and create a directory section where we allow overrides.
This should look something like this:

```
<VirtualHost *:80>
 ServerAdmin webmaster@localhost
 DocumentRoot /var/www/html
 ServerName server_domain_name_or_IP
 <Directory /var/www/html/>
  AllowOverride All
 </Directory>
 . . .
 ```

 Next, we need to enable the rewrite module, which allows you to modify URLs. You can do this by typing:

```bash
sudo a2enmod rewrite
```

Restart apache:

```bash
sudo service apache2 restart
```

#### Create an .htaccess File

Now that Apache is configured to allow rewrites through .htaccess files, we need to create an actual file.

You need to place this file in your document root. Type this to create an empty file:

```bash
touch /var/www/html/.htaccess
```

This will be created with your username and user group. We need the web server to be the group owner though, so we should adjust the ownership by typing:

```bash
sudo chown :www-data /var/www/html/.htaccess
```

If you want WordPress to automatically update this file with rewrite rules, you can ensure that it has the correct permissions to do so by typing:

```bash
chmod 664 /var/www/html/.htaccess
```

#### Change the Permalink Settings in WordPress

When you are finished doing the server-side changes, you can easily adjust the permalink settings through the WordPress administration interface.

On the left-hand side, under the `Settings` menu, you can select `Permalinks`.

When you have made your selection, click `"Save Changes"` to generate the rewrite rules.

If you allowed the web server write access to your .htaccess file, you should see a message like this:

```
Permalink structure updated.
```

If you did not allow the web server write access to your .htaccess file, you will be provided with the rewrite rules you need to add to the file manually.

Copy the lines that WordPress gives you and then edit file on your server:

```bash
vim /var/www/html/.htaccess
```

This should give you the same functionality.

Source: https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-on-ubuntu-14-04


## Installing WordPress in Your Language

On your site server, create a new folder in your /wp-content directory called /languages.

```bash
mkdir wp-content/languages
```

Download the language pack from the translations teams: https://make.wordpress.org/polyglots/teams/

```bash
cd wp-content/languages/
wget "https://downloads.wordpress.org/translation/core/4.3/pt_BR.zip"
```

Unpack the .mo files to the languages folder

```bash
unzip pt_BR.zip
```

Change the language in the admin settings screen. `Settings` > `General` > Site Language.

# Sources

Installing wordpress in your language: https://codex.wordpress.org/Installing_WordPress_in_Your_Language

Language packs: https://make.wordpress.org/polyglots/teams/

PT-BR language pack: https://make.wordpress.org/polyglots/teams/?locale=pt_BR
