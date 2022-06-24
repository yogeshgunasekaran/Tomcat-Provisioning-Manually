# Tomcat-Provisioning-Manually
Installation of Tomcat in CentOS7
### Login to the root user
 ```sh
  sudo su -
 ```
 ### Update OS with latest patches
  ```sh
  yum update -y
  ```
  ### Set EPEL Repository
  ```sh
  yum install epel-release -y 
  ```
  ### Install Dependencies
  ```sh
  yum install java-1.8.0-openjdk -y
  ```
  ```sh
  yum install git maven -y
  ```
  ```sh
  yum install wget -y
  ```
  ### Change dir to /tmp
  ```sh
  cd /tmp/
  ```
  ### Download & Tomcat Package
  ```sh
  wget https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.37/bin/apache-tomcat-8.5.37.tar.gz
  ```
  ```sh
  tar xzvf apache-tomcat-8.5.37.tar.gz
  ```
  ### Add tomcat user
  ```sh
  useradd --home-dir /usr/local/tomcat8 --shell /sbin/nologin tomcat
  ```
  ### Copy data to tomcat home dir
  ```sh
  cp -r /tmp/apache-tomcat-8.5.37/* /usr/local/tomcat8/
  ```
  ### Make tomcat user owner of tomcat home dir
  ```sh
  chown -R tomcat.tomcat /usr/local/tomcat8
  ```
  ### Setup systemd for tomcat
  #### Update file with following content:
  ```sh
  vi /etc/systemd/system/tomcat.service
  ```
  ~~~
  [Unit]
  Description=Tomcat
  After=network.target 
  
  [Service]
  User=tomcat
  WorkingDirectory=/usr/local/tomcat8
  Environment=JRE_HOME=/usr/lib/jvm/jre
  Environment=JAVA_HOME=/usr/lib/jvm/jre
  Environment=CATALINA_HOME=/usr/local/tomcat8
  Environment=CATALINE_BASE=/usr/local/tomcat8
  ExecStart=/usr/local/tomcat8/bin/catalina.sh run
  ExecStop=/usr/local/tomcat8/bin/shutdown.sh
  SyslogIdentifier=tomcat-%i 
  
  [Install]
  WantedBy=multi-user.target
  ~~~
  ```sh
  systemctl daemon-reload
  ```
  ```sh
  systemctl start tomcat
  ```
  ```sh
  systemctl enable tomcat
  ```
  ### Enabling the firewall and allowing port 8080 to access the tomcat (Optional)
  ```sh
  systemctl start firewalld
  ```
  ```sh
  systemctl enable firewalld
  ```
  ```sh
  firewall-cmd --get-active-zones
  ```
  ```sh
  firewall-cmd --zone=public --add-port=8080/tcp --permanent
  ```
  ```sh
  firewall-cmd --reload
  ```
  
 
