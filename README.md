
IaC HW4
.... ......

........... ansible-.... ... ......... WordPress . .............. .............., ........ ... ...... .......... ...........
..........

...... ......... .... .......... .............. .. ...... Ansible 2.9.

.. . ............. ............. .......... ...... . ........... ........ . .. ...... ....... Ansible, .. .. ..............

.......... .. ......... ..... ..... ...: https://docs.ansible.com/ansible/2.9/installation_guide/intro_installation.html
............. .......... .........

... ... . WordPress . ........... ............ ... ............ .... .... /health, .. ... ........ .......... ..... . attached_target_group . ..... lb.tf .... ........ .... healthcheck .. .........:

    healthcheck {
      name = "tcp"
      tcp_options {
        port = 80
      }
    }

. . .... output.tf ....... ..... ....... IP, .. ... ...........:

output "vm_linux_2_public_ip_address" {
  description = "Virtual machine IP"
  value = yandex_compute_instance.wp-app-2.network_interface[0].nat_ip_address
}

........ ......... ... ...... .......:

. terraform apply --auto-approve

.......... ........

...... ..... ........ ....... ansible. .......... ....... ... .. ..... ...... . ......... terraform:

. mkdir ansible
 
. ls -lh

-rw-r--r--   1 igor  staff    26B Sep 23 08:17 README.md
drwxr-xr-x   8 igor  staff   256B Sep 23 22:37 ansible
drwxr-xr-x  16 igor  staff   512B Sep 24 18:37 terraform

........ . ....... ansible . ........ ........... ......... ............

. mkdir -p environments/prod/group_vars
. mkdir files
. mkdir templates
. mkdir playbooks
. mkdir roles

.............. .... ......... .........:

    . environments ..... ......... ......... . ........ ......... .......... ... ............ ..... .............. . ..... ...... ... ..... prod - .... ........... ......
    . files ..... ......... ....., ....... .. ..... ............ . .......... ....
    . templates ..... ......... .......
    . playbooks ..... ............ ........ Ansible
    ....... roles ............ ... ........ .....

..... .. ........ .... ansible.cfg . ......... ........ Ansible.
........ ........ .. ........ ......... private_key_file. .... ... ......... .... .......... .. ....... - ....... ... ...... ......

ansible.cfg

[defaults]
# ...... ..... ......... ..-.........
inventory = ./environments/prod/inventory
# ... ..... ............. ............ . ......
remote_user = ubuntu
# ... ..... ......... .... ... ........... . ......
private_key_file = ~/.ssh/yc
# .......... ........ SSH Host-keys
host_key_checking = False
# .......... ............. *.retry-......
retry_files_enabled = False
# ............... .....
roles_path = ./roles

...... . ..... environments/prod/inventory IP ......, .. ....... .. ..... ....... WordPress.

......... .... IP ............ ... ......., ........ ... ..... ...... .. ....... ..... .. output-.......... ... ........... .. . ...-....... ............. (. ...-....... ... ..... ..... ....... IP!!!)

inventory

[wp_app]
app ansible_host=62.84.112.16
app2 ansible_host=62.84.121.247

....... .. ..........

........... ............ ......... ...... ....... ........ IP ...... . ......... .......

....., ... .... ....... .... . .......... ........... ... ...... ...... wp_app . ........ environments/prod/group_vars

.......... FQDN ... ..... .... ...... .. ....... ... ...... .......... .. .............: https://cloud.yandex.com/en-ru/docs/managed-mysql/operations/connect#fqdn-master
ID ........ MySQL ..... ..... ...-....... .............:

... .... ......, ... ............ .... ...... . ... ...... .. ......... . ............... ......... ...........

wp_app

wordpress_db_name: db
wordpress_db_user: user
wordpress_db_password: password
wordpress_db_host: c-c9q304kesgmvb1raujqu.rw.mdb.yandexcloud.net

...... ..... .......... . ......... ........ ... ......... WordPress. ..... .. ......... ... . .....

. ........ playbooks ........ .... install.yml. . ... ....... ..... .......... . ......... ........... ............, ....... ........... WordPress-..

install.yml

- hosts: wp_app
  become: yes

  tasks:
    - name: Update apt-get repo and cache
      apt: update_cache=yes force_apt_get=yes cache_valid_time=3600

    - name: install dependencies
      package:
        name:
          - apache2
          - ghostscript
          - libapache2-mod-php
          - mysql-server
          - php
          - php-bcmath
          - php-curl
          - php-imagick
          - php-intl
          - php-json
          - php-mbstring
          - php-mysql
          - php-xml
          - php-zip
        state: present

........ ........, ... . ..... ...... .. ......... ...... ...... .. ...... ........., ... ....... ..... ......... ......... . ...... ........ ......:

- hosts: wp_app

........ ....... . ........, ... ........... . ......... ...... ...... ....... . ......... ....... ...... ... .......:

. ansible-playbook playbooks/install.yml

PLAY [wp_app] *********************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************************
ok: [app]
ok: [app2]

TASK [Update apt-get repo and cache] **********************************************************************************************************
changed: [app]
changed: [app2]

TASK [install dependencies] *******************************************************************************************************************
changed: [app2]
changed: [app]

......... ... ... - ... ........ .. ...... ........, .... ..... ........... WordPress . .......... ............ WordPress . ................. . .... ........

....... ......... .... . ... .......

    - name: Create the installation directory
      file:
        path: /srv/www
        owner: www-data
        state: directory

    - name: Install WordPress
      unarchive:
        src: https://wordpress.org/latest.tar.gz
        dest: /srv/www
        owner: www-data
        remote_src: yes

........ ........, ... . ...... file, ..... .... ............ ........, .. ............ ......... . ... ..... ... ........... .. ... ........ ........ owner.

.. .......... . ................ ............ ........ .... ...... - unarchive.

..... ... ... ......... ....... . ........., ... ..... .... ...... .. ........:

. ansible-playbook playbooks/install.yml

... ......... ... - .......... ........ ............ ..... ... ....... Apache2, ..... .......... .. 80-.. .... ...... ........... .. WordPress.

. ...., . ...... ...... . ... .. ..... ............. ...-.. ...... . .....-............, .. ...-.... ..... .......... . ... ... . ......... .... .....-...... ..... ... ............ ............. ............... .... ......, .. ..... ..... ....... ...... .... ...., . ......... ........ ... .... .. ..... ..............

...., . ........ templates ........ .... wordpress.conf.j2 .. ......... ..........:

<VirtualHost *:80>
    DocumentRoot /srv/www/wordpress
    <Directory /srv/www/wordpress>
        Options FollowSymLinks
        AllowOverride Limit Options FileInfo
        DirectoryIndex index.php
        Require all granted
    </Directory>
    <Directory /srv/www/wordpress/wp-content>
        Options FollowSymLinks
        Require all granted
    </Directory>
</VirtualHost>

..... ..... ........ ........ .. ......... .... . ..... - /srv/www/wordpress. ... .. ......., ....... /srv/www .. ......... ..... . .........

. ...... ........... .... ..... ............ ...... . 80-.. ...... . ...... .. .... .... ..... .......... ...... ............., ....... .. ......... . .......... ..........

<VirtualHost *:80>

..... ...., ... .. ........... ... ............, ....... ... . .......:

    - name: Copy file with owner and permissions
      template:
        src: ../templates/wordpress.conf.j2
        dest: /etc/apache2/sites-available/wordpress.conf
        owner: "www-data"

..... .... ... . ............ Apache ......... ........ ...... ....., .......... ......... ... .............. .....:

    ........ . Apache ...... rewrite
    .............. ... ....
    ................ ....-........ (.It Works.)

...... . .......... ...... .. ..... ...... ... ...... ansible-...... apache2_module. ... ......... ..... ........ ............. ansible-...... shell.

....... ..... ...... . .......:

    - name: Enable URL rewriting
      apache2_module:
        name: rewrite
        state: present

    - name: Enable the site
      shell: a2ensite wordpress

    - name: Disable the default .It Works. site
      shell: a2dissite 000-default

    - name: Reload service apache2
      service:
        name: apache2
        state: reloaded

......... ......, ....... .... ...... . .... ........, ... .......... WordPress . .... ...... MySql.

... ..... ... ........... .......... .. ..... ........ .......... ... .......... . ..... ...... .. SSL. . ... ..... ..... ...... ......... . ............ ... ...... WordPress.

...... . ............ ........ ... . ........ files:

. wget "https://storage.yandexcloud.net/cloud-certs/CA.pem" -O ./files/root.crt

....., . ........ templates ........ .... wp-config.php.j2 .. ......... ..........:

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
 * * MySQL settings
 * * Secret keys
 * * Database table prefix
 * * ABSPATH
 *
 * @link https://wordpress.org/support/article/editing-wp-config-php/
 *
 * @package WordPress
 */

// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', '{{ wordpress_db_name }}' );

/** MySQL database username */
define( 'DB_USER', '{{ wordpress_db_user }}' );

/** MySQL database password */
define( 'DB_PASSWORD', '{{ wordpress_db_password }}' );

/** MySQL hostname */
define( 'DB_HOST', '{{ wordpress_db_host }}' );

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
 * @link https://wordpress.org/support/article/debugging-in-wordpress/
 */
define( 'WP_DEBUG', true );
define( 'WP_DEBUG_LOG', true );

/* Add any custom values between this line and the "stop editing" line. */



/* That's all, stop editing! Happy publishing. */

/** Absolute path to the WordPress directory. */
if ( ! defined( 'ABSPATH' ) ) {
        define( 'ABSPATH', __DIR__ . '/' );
}

define('MYSQL_CLIENT_FLAGS', MYSQLI_CLIENT_SSL);

/** Sets up WordPress vars and included files. */
require_once ABSPATH . 'wp-settings.php';

... ......, ... ... ....... ......, . ....... ..... ............. ........ .. ..........:

    wordpress_db_name
    wordpress_db_user
    wordpress_db_password
    wordpress_db_host

... .. ......., ... ......... .......... .. ..... environments/prod/group_vars/wp_app

..... ..... ..... ........ ........ .. ......... ......:

define('MYSQL_CLIENT_FLAGS', MYSQLI_CLIENT_SSL);

.... .......... .. ........ WordPress, ... ........... . .... ...... ..... .... .. SSL.

....... ......... .... . ... .......:

    - name: Install mysql cert
      copy:
        src: ../files/root.crt
        dest: /usr/local/share/ca-certificates/root.crt

    - name: Update cert index
      shell: /usr/sbin/update-ca-certificates

    - name: Configure WordPress
      template:
        src: ../templates/wp-config.php.j2
        dest: "/srv/www/wordpress/wp-config.php"
        owner: "www-data"

..... .. ... ...... ...... copy ......... .. ..... ........ ...........

..... ... ...... ...... shell ......... .. ...... ......... .............

., ... ...... ...... template, . ....... .. ... ....... (.......... wordpress.conf.j2) ......... ............ . ............ . .... .......

....., ...... ... ....... ...... ......... ...:

- hosts: wp_app
  become: yes

  tasks:
    - name: Update apt-get repo and cache
      apt: update_cache=yes force_apt_get=yes cache_valid_time=3600

    - name: install dependencies
      package:
        name:
          - apache2
          - ghostscript
          - libapache2-mod-php
          - mysql-server
          - php
          - php-bcmath
          - php-curl
          - php-imagick
          - php-intl
          - php-json
          - php-mbstring
          - php-mysql
          - php-xml
          - php-zip
        state: present

    - name: Create the installation directory
      file:
        path: /srv/www
        owner: www-data
        state: directory

    - name: Install WordPress
      unarchive:
        src: https://wordpress.org/latest.tar.gz
        dest: /srv/www
        owner: www-data
        remote_src: yes

    - name: Copy file with owner and permissions
      copy:
        src: ../templates/wordpress.conf.j2
        dest: /etc/apache2/sites-available/wordpress.conf

    - name: Enable URL rewriting
      apache2_module:
        name: rewrite
        state: present

    - name: Enable the site
      shell: a2ensite wordpress

    - name: Disable the default .It Works. site
      shell: a2dissite 000-default

    - name: Reload service apache2
      service:
        name: apache2
        state: reloaded

    - name: Install mysql cert
      copy:
        src: ../files/root.crt
        dest: /usr/local/share/ca-certificates/root.crt

    - name: Update cert index
      shell: /usr/sbin/update-ca-certificates

    - name: Configure WordPress
      template:
        src: ../templates/wp-config.php.j2
        dest: "/srv/www/wordpress/wp-config.php"
        owner: "www-data"

........ ....... ... ...

. ansible-playbook playbooks/install.yml

. ......... ....... ........ . ........ ......... . IP .............. (... ..... ..... . output-.......... ... . ...-....... .............).

.. ...... ...... ....... ........:

........... WordPress .... ... ..............
.............. ........ . ....

...., ...... ..... ...... ........ ...... - .. ...... .............. ........ ....... .....

.......... ......... .........:

. mkdir roles/wordpress
. mkdir roles/wordpress/default
. mkdir roles/wordpress/files
. mkdir roles/wordpress/meta
. mkdir roles/wordpress/tasks
. mkdir roles/wordpress/templates

.........., ....... ... .........., ...... . ..... defaults/main.yml . ...... ..... ........... .........

........ .. ........ .......... ........ . ........ ......... ...........

wordpress_db_name: "mysql_db_name"
wordpress_db_user: "mysql_username"
wordpress_db_password: "mysql_user_password"
wordpress_db_host: "mysql_fqdn"

. ..... meta/main.yml ...... ..... ........ . ..... ....

---
galaxy_info:
  role_name: wordpress
  author: sablin
  description: Wordpress CMS System
  license: "license (MIT)"

  min_ansible_version: 2.9

  galaxy_tags:
    - linux
    - server
    - web
    - wordpress
    - cms

  platforms:
    - name: Ubuntu
      versions:
        - Focal

....., . ....... .... files ......... ........ ...........

. . ....... .... templates ......... ....... wordpress.conf.j2 . wp-config.php.j2.

. ... ........ ......... .... ........ . .... tasks/main.yml.

main.yml

---
- name: Update apt-get repo and cache
  apt: update_cache=yes force_apt_get=yes cache_valid_time=3600

- name: install dependencies
  package:
    name:
      - apache2
      - ghostscript
      - libapache2-mod-php
      - mysql-server
      - php
      - php-bcmath
      - php-curl
      - php-imagick
      - php-intl
      - php-json
      - php-mbstring
      - php-mysql
      - php-xml
      - php-zip
    state: present

- name: Create the installation directory
  file:
    path: /srv/www
    owner: www-data
    state: directory

- name: Install WordPress
  unarchive:
    src: https://wordpress.org/latest.tar.gz
    dest: /srv/www
    owner: www-data
    remote_src: yes

- name: Copy file with owner and permissions
  template:
    src: wordpress.conf.j2
    dest: /etc/apache2/sites-available/wordpress.conf

- name: Enable URL rewriting
  apache2_module:
    name: rewrite
    state: present

- name: Enable the site
  shell: a2ensite wordpress

- name: Disable the default .It Works. site
  shell: a2dissite 000-default

- name: Reload service apache2
  service:
    name: apache2
    state: reloaded

- name: Install mysql cert
  copy:
    src: ./files/root.crt
    dest: /usr/local/share/ca-certificates/root.crt

- name: Update cert index
  shell: /usr/sbin/update-ca-certificates

- name: Configure WordPress
  template:
    src: wp-config.php.j2
    dest: "/srv/www/wordpress/wp-config.php"
    owner: "www-data"

....., ......... ......... .... ...... ......... ...:

. tree
.
... wordpress
    ... defaults
    .   ... main.yml
    ... files
    .   ... root.crt
    ... meta
    .   ... main.yml
    ... tasks
    .   ... main.yml
    ... templates
        ... wordpress.conf.j2
        ... wp-config.php.j2

........ ....... ...... ..... ......... ......... .......:

- hosts: wp_app
  become: yes

  roles:
    - role: wordpress

... ... .. ..... ...... - ... ........ ............... .....

........ ... ... ......., ..... ........., ... ...... .. .........:

. ansible-playbook playbooks/install.yml

.... ....... ......... ... ......, .. ...... ..... ...... .. .... . ......... ... .........:

........, ... ......., .... .......... . ....... .. Continue.

..... ......... My site, Username, Password (... ........ ... ............ . ...... - ... .............. ....., ... .. ............ .... ......), Your Email . ....... .. Install WordPress.

..... ......... ..... ........ ........., ... ......... .........:

. ..... ....... .. Log in. ..... ...., ..... ..... . ...... .............. ....., .. ........ . ... .......:

....... . ....... ..... .... .. ...... My site, .. ....... .. ........ ........ WordPress.

. ...... ..... ..... ....... . ........ ansible ...... ........ - files . templates, ... ... ... ........... ... ......... . .... wordpress.

........ ......... ........ ansible ...... ......... ...:

. tree
.
... ansible.cfg
... environments
.   ... prod
.       ... group_vars
.       .   ... wp_app
.       ... inventory
... playbooks
.   ... install.yml
... roles
    ... wordpress
        ... defaults
        .   ... main.yml
        ... files
        .   ... root.crt
        ... meta
        .   ... main.yml
        ... tasks
        .   ... main.yml
        ... templates
            ... wordpress.conf.j2
            ... wp-config.php.j2

........ ........... .....

............ . ........ .......... ..... . ............. .. ... WordPress .. ...... ........., ... .... ..... ........ .... ........ .... .. .... .....
........ ..

... ........ .. .......... ............ IP .............., ... ......... . ........ ........... .... WordPress.

. ........... ...... ......... .... README.md . ......... ........... .......

............. .......... ......... . .. .... ....... ........
Last changed by  
.
Otus
·
0
Published on HackMD
