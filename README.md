Skip to content
This repository
Search
Pull requests
Issues
Gist
 @Mamadou208
 Watch 118
  Star 496
 Fork 583 yiisoft/yii2-app-advanced
 Code  Issues 4  Pull requests 0  Projects 0  Wiki  Pulse  Graphs
Branch: master Find file Copy pathyii2-app-advanced/docs/guide/start-installation.md
974b233  12 days ago
@SilverFire SilverFire Updated fxp/ require version
12 contributors @samdark @thiagotalma @cebe @SilverFire @saada @rugabarbo @janisto @glagola @haobingwang @softark @CedricYii @ashatrov-lazada
RawBlameHistory     280 lines (203 sloc)  9.54 KB
Installation

Requirements

The minimum requirement by this project template is that your Web server supports PHP 5.4.0.

Installing using Composer

If you do not have Composer, follow the instructions in the Installing Yii section of the definitive guide to install it.

With Composer installed, you can then install the application using the following commands:

composer global require "fxp/composer-asset-plugin:^1.2.0"
composer create-project --prefer-dist yiisoft/yii2-app-advanced yii-application
The first command installs the composer asset plugin which allows managing bower and npm package dependencies through Composer. You only need to run this command once for all. The second command installs the advanced application in a directory named yii-application. You can choose a different directory name if you want.

Install from an Archive File

Extract the archive file downloaded from yiiframework.com to a directory named advanced that is directly under the Web root.

Then follow the instructions given in the next subsection.

Preparing application

After you install the application, you have to conduct the following steps to initialize the installed application. You only need to do these once for all.

Open a console terminal, execute the init command and select dev as environment.

/path/to/php-bin/php /path/to/yii-application/init
If you automate it with a script you can execute init in non-interactive mode.

/path/to/php-bin/php /path/to/yii-application/init --env=Production --overwrite=All
Create a new database and adjust the components['db'] configuration in common/config/main-local.php accordingly.

Open a console terminal, apply migrations with command /path/to/php-bin/php /path/to/yii-application/yii migrate.

Set document roots of your web server:

for frontend /path/to/yii-application/frontend/web/ and using the URL http://frontend.dev/
for backend /path/to/yii-application/backend/web/ and using the URL http://backend.dev/
For Apache it could be the following:

   <VirtualHost *:80>
       ServerName frontend.dev
       DocumentRoot "/path/to/yii-application/frontend/web/"

       <Directory "/path/to/yii-application/frontend/web/">
           # use mod_rewrite for pretty URL support
           RewriteEngine on
           # If a directory or a file exists, use the request directly
           RewriteCond %{REQUEST_FILENAME} !-f
           RewriteCond %{REQUEST_FILENAME} !-d
           # Otherwise forward the request to index.php
           RewriteRule . index.php

           # use index.php as index file
           DirectoryIndex index.php

           # ...other settings...
       </Directory>
   </VirtualHost>

   <VirtualHost *:80>
       ServerName backend.dev
       DocumentRoot "/path/to/yii-application/backend/web/"

       <Directory "/path/to/yii-application/backend/web/">
           # use mod_rewrite for pretty URL support
           RewriteEngine on
           # If a directory or a file exists, use the request directly
           RewriteCond %{REQUEST_FILENAME} !-f
           RewriteCond %{REQUEST_FILENAME} !-d
           # Otherwise forward the request to index.php
           RewriteRule . index.php

           # use index.php as index file
           DirectoryIndex index.php

           # ...other settings...
       </Directory>
   </VirtualHost>
For nginx:

   server {
       charset utf-8;
       client_max_body_size 128M;

       listen 80; ## listen for ipv4
       #listen [::]:80 default_server ipv6only=on; ## listen for ipv6

       server_name frontend.dev;
       root        /path/to/yii-application/frontend/web/;
       index       index.php;

       access_log  /path/to/yii-application/log/frontend-access.log;
       error_log   /path/to/yii-application/log/frontend-error.log;

       location / {
           # Redirect everything that isn't a real file to index.php
           try_files $uri $uri/ /index.php$is_args$args;
       }

       # uncomment to avoid processing of calls to non-existing static files by Yii
       #location ~ \.(js|css|png|jpg|gif|swf|ico|pdf|mov|fla|zip|rar)$ {
       #    try_files $uri =404;
       #}
       #error_page 404 /404.html;

       # deny accessing php files for the /assets directory
       location ~ ^/assets/.*\.php$ {
           deny all;
       }

       location ~ \.php$ {
           include fastcgi_params;
           fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
           fastcgi_pass 127.0.0.1:9000;
           #fastcgi_pass unix:/var/run/php5-fpm.sock;
           try_files $uri =404;
       }

       location ~* /\. {
           deny all;
       }
   }

   server {
       charset utf-8;
       client_max_body_size 128M;

       listen 80; ## listen for ipv4
       #listen [::]:80 default_server ipv6only=on; ## listen for ipv6

       server_name backend.dev;
       root        /path/to/yii-application/backend/web/;
       index       index.php;

       access_log  /path/to/yii-application/log/backend-access.log;
       error_log   /path/to/yii-application/log/backend-error.log;

       location / {
           # Redirect everything that isn't a real file to index.php
           try_files $uri $uri/ /index.php$is_args$args;
       }

       # uncomment to avoid processing of calls to non-existing static files by Yii
       #location ~ \.(js|css|png|jpg|gif|swf|ico|pdf|mov|fla|zip|rar)$ {
       #    try_files $uri =404;
       #}
       #error_page 404 /404.html;

       # deny accessing php files for the /assets directory
       location ~ ^/assets/.*\.php$ {
           deny all;
       }

       location ~ \.php$ {
           include fastcgi_params;
           fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
           fastcgi_pass 127.0.0.1:9000;
           #fastcgi_pass unix:/var/run/php5-fpm.sock;
           try_files $uri =404;
       }

       location ~* /\. {
           deny all;
       }
   }
Change the hosts file to point the domain to your server.

Windows: c:\Windows\System32\Drivers\etc\hosts
Linux: /etc/hosts
Add the following lines:

127.0.0.1   frontend.dev
127.0.0.1   backend.dev
To login into the application, you need to first sign up, with any of your email address, username and password. Then, you can login into the application with same email address and password at any time.

Note: if you want to run advanced template on a single domain so / is frontend and /admin is backend, refer to Using advanced project template at shared hosting.
Installing using Vagrant

This way is the easiest but long (~20 min).

This installation way doesn't require pre-installed software (such as web-server, PHP, MySQL etc.) - just do next steps!

Manual for Linux/Unix users

Install VirtualBox
Install Vagrant
Create GitHub personal API token
Prepare project:

git clone https://github.com/yiisoft/yii2-app-advanced.git
cd yii2-app-advanced/vagrant/config
cp vagrant-local.example.yml vagrant-local.yml
Place your GitHub personal API token to vagrant-local.yml

Change directory to project root:

cd yii2-app-advanced
Run commands:

vagrant plugin install vagrant-hostmanager
vagrant up
That's all. You just need to wait for completion! After that you can access project locally by URLs:

frontend: http://y2aa-frontend.dev
backend: http://y2aa-backend.dev
Manual for Windows users

Install VirtualBox
Install Vagrant
Reboot
Create GitHub personal API token
Prepare project:

download repo yii2-app-advanced
unzip it
go into directory yii2-app-advanced-master/vagrant/config
copy vagrant-local.example.yml to vagrant-local.yml
Place your GitHub personal API token to vagrant-local.yml

Add the following lines to hosts file:

192.168.83.137 y2aa-frontend.dev
192.168.83.137 y2aa-backend.dev
Open terminal (cmd.exe), change directory to project root and run commands:

vagrant plugin install vagrant-hostmanager
vagrant up
(You can read here how to change directories in command prompt)

That's all. You just need to wait for completion! After that you can access project locally by URLs:

frontend: http://y2aa-frontend.dev
backend: http://y2aa-backend.dev
Contact GitHub API Training Shop Blog About
© 2016 GitHub, Inc. Terms Privacy Security Status Help
