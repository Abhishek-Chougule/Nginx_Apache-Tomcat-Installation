# Nginx_Apache-Tomcat-Installation
=================================================================================================================================================================================================================

=================================================================================================================================================================================================================

<!-- Contributed by Abhishek Chougule -->
<!-- Documentation -->
<!-- Installation of Nginx and Apache Tomcat -->

=================================================================================================================================================================================================================
										NGINX
=================================================================================================================================================================================================================


sudo apt update
#sudo apt install nginx
sudo ufw app list
#sudo ufw allow 'Nginx HTTP'
#systemctl status nginx
curl -4 icanhazip.com
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
sudo systemctl disable nginx
sudo systemctl enable nginx

sudo mkdir -p /var/www/your_domain/html
sudo chown -R $USER:$USER /var/www/your_domain/html
sudo chmod -R 755 /var/www/your_domain

===========================================================================================

sudo nano /var/www/your_domain/html/index.html
<html>
    <head>
        <title>Welcome to your_domain!</title>
    </head>
    <body>
        <h1>Success!  The your_domain server block is working!</h1>
    </body>
</html>

===========================================================================================

sudo nano /etc/nginx/sites-available/your_domain
server {
        listen 80;
        listen [::]:80;

        root /var/www/your_domain/html;
        index index.html index.htm index.nginx-debian.html;

        server_name your_domain www.your_domain;

        location / {
                try_files $uri $uri/ =404;
        }
}

===========================================================================================

sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/

===========================================================================================

sudo nano /etc/nginx/nginx.conf
http {
    ...
    server_names_hash_bucket_size 64;
    ...
}

===========================================================================================

sudo nginx -t
sudo systemctl restart nginx
http://your_domain




=================================================================================================================================================================================================================
=================================================================================================================================================================================================================
=================================================================================================================================================================================================================
=================================================================================================================================================================================================================








=================================================================================================================================================================================================================
									     Apache TomCat
=================================================================================================================================================================================================================

sudo apt install default-jdk -y
wget https://archive.apache.org/dist/tomcat/tomcat-10/v10.0.8/bin/apache-tomcat-10.0.8.tar.gz
sudo tar xzvf apache-tomcat-10.0.8.tar.gz
sudo mkdir /opt/tomcat/
sudo mv apache-tomcat-10.0.8/* /opt/tomcat/
sudo chown -R www-data:www-data /opt/tomcat/
sudo chmod -R 755 /opt/tomcat/
sudo nano /opt/tomcat/conf/tomcat-users.xml
add to <tomcat-users>

===========================================================================================

<!-- user manager can access only manager section -->

<role rolename="manager-gui" />

<user username="manager" password="StrongPassword" roles="manager-gui" />



<!-- user admin can access manager and admin section both -->

<role rolename="admin-gui" />

<user username="admin" password="StrongPassword" roles="manager-gui,admin-gui" />

===========================================================================================


sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml
sudo nano /opt/tomcat/webapps/host-manager/META-INF/context.xml
sudo nano /etc/systemd/system/tomcat.service

===========================================================================================

[Unit]

Description=Tomcat

After=network.target



[Service]

Type=forking



User=root

Group=root



Environment="JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64"

Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom"

Environment="CATALINA_BASE=/opt/tomcat"

Environment="CATALINA_HOME=/opt/tomcat"

Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"

Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"



ExecStart=/opt/tomcat/bin/startup.sh

ExecStop=/opt/tomcat/bin/shutdown.sh



[Install]

WantedBy=multi-user.target

===========================================================================================

sudo systemctl daemon-reload
sudo systemctl start tomcat
sudo systemctl enable tomcat
sudo systemctl status tomcat
http://ServerIPaddress:8080


=================================================================================================================================================================================================================
=================================================================================================================================================================================================================

									      <!-- End -->
<!-- 								      Contributed by Abhishek Chougule 									     -->
<!-- 									       Thank You ! 										     -->
=================================================================================================================================================================================================================
=================================================================================================================================================================================================================
