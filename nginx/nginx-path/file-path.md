++ Important NGINX file path ++

Path	                         Purpose
/etc/nginx/nginx.conf	         Main configuration file used to manage global NGINX settings.
/etc/nginx/sites-available/	     Stores website configuration files, including enabled and disabled websites.
/etc/nginx/sites-enabled/	     Enabled Websites ( nginx will enable the file that have in this path )
/var/www/html/	                 Default Website Directory
/var/log/nginx/access.log	     Access Logs
/var/log/nginx/error.log	     Error Logs