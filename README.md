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
  yum install git maven wget -y
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
  ### Start & Enable Tomcat
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
  <br>
  
  # To Build and Deploy Maven Artifact (.WAR) in this Tomcat Server
  ### Download Source code from Github
  ```sh
  git clone -b <branch name> <repository link>
  ```
  ### Update configuration for Backend Servers in the Project
  ```sh
  cd <project directory been cloned>  
  ```
  ```sh
  vi src/main/resources/application.properties
  ```
  > Edit this "application.properties" file with all the backend servers details
~~~
#JDBC Configutation for Database Connection
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://<enter mysql server ip here>:3306/accounts?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull
jdbc.username=<enter mysql user name here>
jdbc.password=<enter mysql user password here>

#Memcached Configuration For Active and StandBy Host
#For Active Host
memcached.active.host=<enter memcached server ip here>
memcached.active.port=11211
#For StandBy Host
memcached.standBy.host=<<enter memcached standBy servers (if any) ip here>
memcached.standBy.port=11211

#RabbitMq Configuration
rabbitmq.address=<enter rabbitmq server ip here>
rabbitmq.port=5672
rabbitmq.username=<enter rabbitmq user name here>
rabbitmq.password=<enter rabbitmq user password here>

#Elasticesearch Configuration
elasticsearch.host =<enter elasticsearch server ip here>
elasticsearch.port =9300
elasticsearch.cluster=<enter cluster name here>
elasticsearch.node=<enter clusternode name here>
~~~
 
  
  ### Build code
  #### Run below command inside the project directory that contains .POM file in it,
  ```sh
  mvn install 
  ```
  ```sh
  
  ```
  
 
