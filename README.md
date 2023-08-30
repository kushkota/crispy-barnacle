# crispy-barnacle
Install LAMP on Amazon Linux 2023


$ sudo dnf update -y   
        
$ sudo dnf install -y httpd
        ^--- Amazon Linux release 2023 (Amazon Linux)

// reason to use dnf
$ cat /etc/system-release => Amazon Linux release 2023 (Amazon Linux)

// start httpd
$ sudo systemctl start httpd

// boot
$ sudo systemctl enable httpd

// verification
$ sudo systemctl is-enabled httpd => enabled

// VERIFICATION
$ sudo systemctl status httpd
● httpd.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; preset: disabled)
    Drop-In: /usr/lib/systemd/system/httpd.service.d
             └─php-fpm.conf
     Active: active (running) since Wed 2023-08-30 03:35:45 UTC; 26min ago
       Docs: man:httpd.service(8)
   Main PID: 38808 (httpd)


Apache httpd serves files that are kept in a directory called the Apache document root. 
The Amazon Linux Apache document root is /var/www/html, which by default is owned by root.


$ apachectl -V
Server version: Apache/2.4.56 (Amazon Linux)

Server compiled with....
 -D APR_HAS_SENDFILE
 -D APR_HAS_MMAP
 -D APR_HAVE_IPV6 (IPv4-mapped addresses enabled)
 -D APR_USE_PROC_PTHREAD_SERIALIZE
 -D APR_USE_PTHREAD_SERIALIZE
 -D SINGLE_LISTEN_UNSERIALIZED_ACCEPT
 -D APR_HAS_OTHER_CHILD
 -D AP_HAVE_RELIABLE_PIPED_LOGS
 -D DYNAMIC_MODULE_LIMIT=256
 -D HTTPD_ROOT="/etc/httpd"
 -D SUEXEC_BIN="/usr/sbin/suexec"
 -D DEFAULT_PIDLOG="run/httpd.pid"
 -D DEFAULT_SCOREBOARD="logs/apache_runtime_status"
 -D DEFAULT_ERRORLOG="logs/error_log"
 -D AP_TYPES_CONFIG_FILE="conf/mime.types"
 -D SERVER_CONFIG_FILE="conf/httpd.conf"


 Finding default root directory for your Apache web server

-D HTTPD_ROOT="/etc/httpd" + -D SERVER_CONFIG_FILE="conf/httpd.conf"

ServerRoot "/etc/httpd"

DocumentRoot "/var/www/html"


conf]$ grep -Ri DocumentRoot .
./httpd.conf:# DocumentRoot: The directory out of which you will serve your
./httpd.conf:DocumentRoot "/var/www/html"


$ sudo usermod -a -G apache ec2-user

$ exit

$ sudo chown -R ec2-user:apache /var/www

$ sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;

$ find /var/www -type f -exec sudo chmod 0664 {} \;

ec2-user]# tail -f /var/log/httpd/access_log
127.0.0.1 - - [30/Aug/2023:03:35:58 +0000] "GET / HTTP/1.1" 403 45 "-" "curl/8.0.1"

It seems like you are encountering an error message while trying to serve a directory in your Apache server. The error message indicates that there is no matching DirectoryIndex file (such as index.html or index.php) found in the directory, and the server-generated directory index is forbidden by the Options directive 1.



$  echo '<b>Hello! It is working!</b>' > /var/www/html/index.html
127.0.0.1 - - [30/Aug/2023:04:16:45 +0000] "GET / HTTP/1.1" 200 29 "-" "curl/8.0.1"



