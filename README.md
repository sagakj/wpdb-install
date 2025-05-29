# wpdb-install
Installation commands for Wordpress and MariaDB
$ sudo yum install httpd php-fpm mariadb105-server -y

$ sudo systemctl restart httpd.service mariadb.service php-fpm.service

$ sudo systemctl enable httpd.service mariadb.service php-fpm.service
mariadb setup
[ec2-user@ip-172-31-38-198 ~]$ sudo mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] n
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
Creating user,db for wordpress
[ec2-user@ip-172-31-38-198 ~]$ mysql -u root -pmysqlroot123
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 13
Server version: 10.5.25-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> create database wordpress;
Query OK, 1 row affected (0.000 sec)

MariaDB [(none)]> create user 'wordpress'@'localhost' identified by 'wordpress';
Query OK, 0 rows affected (0.001 sec)

MariaDB [(none)]> grant all privileges on wordpress.* to 'wordpress'@'localhost';
Query OK, 0 rows affected (0.002 sec)

MariaDB [(none)]> flush privileges;
Query OK, 0 rows affected (0.000 sec)
[ec2-user@ip-172-31-38-198 ~]$ mysql -u wordpress -pwordpress
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 14
Server version: 10.5.25-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| wordpress          |
+--------------------+
2 rows in set (0.000 sec)

MariaDB [(none)]> exit
Bye
Downloading WordPress
wget https://wordpress.org/wordpress-6.4.tar.gz
tar -xvf wordpress-6.4.tar.gz
sudo cp -r wordpress/* /var/www/html/
sudo chown -R apache:apache /var/www/html/*

[ec2-user@ip-172-31-38-198 html]$ ls -l
total 240
-rw-r--r--.  1 apache apache   405 May 12 05:24 index.php
-rw-r--r--.  1 apache apache 19915 May 12 05:24 license.txt
-rw-r--r--.  1 apache apache  7399 May 12 05:24 readme.html
-rw-r--r--.  1 apache apache  7211 May 12 05:24 wp-activate.php
drwxr-xr-x.  9 apache apache 16384 May 12 05:24 wp-admin
-rw-r--r--.  1 apache apache   351 May 12 05:24 wp-blog-header.php
-rw-r--r--.  1 apache apache  2323 May 12 05:24 wp-comments-post.php
-rw-r--r--.  1 apache apache  3013 May 12 05:24 wp-config-sample.php
drwxr-xr-x.  4 apache apache    52 May 12 05:24 wp-content
-rw-r--r--.  1 apache apache  5638 May 12 05:24 wp-cron.php
drwxr-xr-x. 27 apache apache 16384 May 12 05:24 wp-includes
-rw-r--r--.  1 apache apache  2502 May 12 05:24 wp-links-opml.php
-rw-r--r--.  1 apache apache  3927 May 12 05:24 wp-load.php
-rw-r--r--.  1 apache apache 50924 May 12 05:24 wp-login.php
-rw-r--r--.  1 apache apache  8525 May 12 05:24 wp-mail.php
-rw-r--r--.  1 apache apache 26409 May 12 05:24 wp-settings.php
-rw-r--r--.  1 apache apache 34385 May 12 05:24 wp-signup.php
-rw-r--r--.  1 apache apache  4885 May 12 05:24 wp-trackback.php
-rw-r--r--.  1 apache apache  3154 May 12 05:24 xmlrpc.php
[ec2-user@ip-172-31-38-198 html]$
[ec2-user@ip-172-31-38-198 html]$ sudo cp -p wp-config-sample.php wp-config.php
[ec2-user@ip-172-31-38-198 html]$
[ec2-user@ip-172-31-38-198 html]$ ls -l
total 244
-rw-r--r--.  1 apache apache   405 May 12 05:24 index.php
-rw-r--r--.  1 apache apache 19915 May 12 05:24 license.txt
-rw-r--r--.  1 apache apache  7399 May 12 05:24 readme.html
-rw-r--r--.  1 apache apache  7211 May 12 05:24 wp-activate.php
drwxr-xr-x.  9 apache apache 16384 May 12 05:24 wp-admin
-rw-r--r--.  1 apache apache   351 May 12 05:24 wp-blog-header.php
-rw-r--r--.  1 apache apache  2323 May 12 05:24 wp-comments-post.php
-rw-r--r--.  1 apache apache  3013 May 12 05:24 wp-config-sample.php
-rw-r--r--.  1 apache apache  3013 May 12 05:24 wp-config.php
drwxr-xr-x.  4 apache apache    52 May 12 05:24 wp-content
-rw-r--r--.  1 apache apache  5638 May 12 05:24 wp-cron.php
drwxr-xr-x. 27 apache apache 16384 May 12 05:24 wp-includes
-rw-r--r--.  1 apache apache  2502 May 12 05:24 wp-links-opml.php
-rw-r--r--.  1 apache apache  3927 May 12 05:24 wp-load.php
-rw-r--r--.  1 apache apache 50924 May 12 05:24 wp-login.php
-rw-r--r--.  1 apache apache  8525 May 12 05:24 wp-mail.php
-rw-r--r--.  1 apache apache 26409 May 12 05:24 wp-settings.php
-rw-r--r--.  1 apache apache 34385 May 12 05:24 wp-signup.php
-rw-r--r--.  1 apache apache  4885 May 12 05:24 wp-trackback.php
-rw-r--r--.  1 apache apache  3154 May 12 05:24 xmlrpc.php
[ec2-user@ip-172-31-38-198 ~]$ cd /var/www/html/
[ec2-user@ip-172-31-38-198 html]$
[ec2-user@ip-172-31-38-198 html]$ ls -l
total 240
-rw-r--r--.  1 apache apache   405 May 12 05:24 index.php
-rw-r--r--.  1 apache apache 19915 May 12 05:24 license.txt
-rw-r--r--.  1 apache apache  7399 May 12 05:24 readme.html
-rw-r--r--.  1 apache apache  7211 May 12 05:24 wp-activate.php
drwxr-xr-x.  9 apache apache 16384 May 12 05:24 wp-admin
-rw-r--r--.  1 apache apache   351 May 12 05:24 wp-blog-header.php
-rw-r--r--.  1 apache apache  2323 May 12 05:24 wp-comments-post.php
-rw-r--r--.  1 apache apache  3013 May 12 05:24 wp-config-sample.php
drwxr-xr-x.  4 apache apache    52 May 12 05:24 wp-content
-rw-r--r--.  1 apache apache  5638 May 12 05:24 wp-cron.php
drwxr-xr-x. 27 apache apache 16384 May 12 05:24 wp-includes
-rw-r--r--.  1 apache apache  2502 May 12 05:24 wp-links-opml.php
-rw-r--r--.  1 apache apache  3927 May 12 05:24 wp-load.php
-rw-r--r--.  1 apache apache 50924 May 12 05:24 wp-login.php
-rw-r--r--.  1 apache apache  8525 May 12 05:24 wp-mail.php
-rw-r--r--.  1 apache apache 26409 May 12 05:24 wp-settings.php
-rw-r--r--.  1 apache apache 34385 May 12 05:24 wp-signup.php
-rw-r--r--.  1 apache apache  4885 May 12 05:24 wp-trackback.php
-rw-r--r--.  1 apache apache  3154 May 12 05:24 xmlrpc.php
[ec2-user@ip-172-31-38-198 html]$
[ec2-user@ip-172-31-38-198 html]$ sudo cp -p wp-config-sample.php wp-config.php
[ec2-user@ip-172-31-38-198 html]$
[ec2-user@ip-172-31-38-198 html]$ ls -l
total 244
-rw-r--r--.  1 apache apache   405 May 12 05:24 index.php
-rw-r--r--.  1 apache apache 19915 May 12 05:24 license.txt
-rw-r--r--.  1 apache apache  7399 May 12 05:24 readme.html
-rw-r--r--.  1 apache apache  7211 May 12 05:24 wp-activate.php
drwxr-xr-x.  9 apache apache 16384 May 12 05:24 wp-admin
-rw-r--r--.  1 apache apache   351 May 12 05:24 wp-blog-header.php
-rw-r--r--.  1 apache apache  2323 May 12 05:24 wp-comments-post.php
-rw-r--r--.  1 apache apache  3013 May 12 05:24 wp-config-sample.php
-rw-r--r--.  1 apache apache  3013 May 12 05:24 wp-config.php
drwxr-xr-x.  4 apache apache    52 May 12 05:24 wp-content
-rw-r--r--.  1 apache apache  5638 May 12 05:24 wp-cron.php
drwxr-xr-x. 27 apache apache 16384 May 12 05:24 wp-includes
-rw-r--r--.  1 apache apache  2502 May 12 05:24 wp-links-opml.php
-rw-r--r--.  1 apache apache  3927 May 12 05:24 wp-load.php
-rw-r--r--.  1 apache apache 50924 May 12 05:24 wp-login.php
-rw-r--r--.  1 apache apache  8525 May 12 05:24 wp-mail.php
-rw-r--r--.  1 apache apache 26409 May 12 05:24 wp-settings.php
-rw-r--r--.  1 apache apache 34385 May 12 05:24 wp-signup.php
-rw-r--r--.  1 apache apache  4885 May 12 05:24 wp-trackback.php
-rw-r--r--.  1 apache apache  3154 May 12 05:24 xmlrpc.php
[ec2-user@ip-172-31-38-198 html]$
[ec2-user@ip-172-31-38-198 html]$
[ec2-user@ip-172-31-38-198 html]$ sudo vim wp-config.php
[ec2-user@ip-172-31-38-198 html]$
[ec2-user@ip-172-31-38-198 html]$
[ec2-user@ip-172-31-38-198 html]$ sudo cat wp-config.php
<?php
/**
 * The base configuration for WordPress
 *
 * The wp-config.php creation script uses this file during the installation.
 * You don't have to use the web site, you can copy this file to "wp-config.php"
 * and fill in the values.
 *
 * This file contains the following configurations:
 *
 * * Database settings
 * * Secret keys
 * * Database table prefix
 * * ABSPATH
 *
 * @link https://wordpress.org/documentation/article/editing-wp-config-php/
 *
 * @package WordPress
 */

// ** Database settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** Database username */
define( 'DB_USER', 'wordpress' );

/** Database password */
define( 'DB_PASSWORD', 'wordpress' );

/** Database hostname */
define( 'DB_HOST', 'localhost' );

/** Database charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );

/** The database collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );

/**#@+
 * Authentication unique keys and salts.
 *
 * Change these to different unique phrases! You can generate these using
 * the {@link https://api.wordpress.org/secret-key/1.1/salt/ WordPress.org secret-key service}.
 *
 * You can change these at any point in time to invalidate all existing cookies.
 * This will force all users to have to log in again.
 *
 * @since 2.6.0
 */
define( 'AUTH_KEY',         'put your unique phrase here' );
define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
define( 'NONCE_KEY',        'put your unique phrase here' );
define( 'AUTH_SALT',        'put your unique phrase here' );
define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
define( 'NONCE_SALT',       'put your unique phrase here' );

/**#@-*/

/**
 * WordPress database table prefix.
 *
 * You can have multiple installations in one database if you give each
 * a unique prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'wp_';

/**
 * For developers: WordPress debugging mode.
 *
 * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 *
 * For information on other constants that can be used for debugging,
 * visit the documentation.
 *
 * @link https://wordpress.org/documentation/article/debugging-in-wordpress/
 */
define( 'WP_DEBUG', false );

/* Add any custom values between this line and the "stop editing" line. */



/* That's all, stop editing! Happy publishing. */

/** Absolute path to the WordPress directory. */
if ( ! defined( 'ABSPATH' ) ) {
        define( 'ABSPATH', __DIR__ . '/' );
}

/** Sets up WordPress vars and included files. */
require_once ABSPATH . 'wp-settings.php';
Error
Your PHP installation appears to be missing the MySQL extension which is required by WordPress. Please check that the mysqli PHP extension is installed and enabled. If you are unsure what these terms mean you should probably contact your host. If you still need help you can always visit the WordPress support forums.
]$ sudo yum install php-mysqlnd

$  sudo systemctl restart httpd.service mariadb.service php-fpm.service
