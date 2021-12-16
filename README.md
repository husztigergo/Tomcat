# Tomcat installation
### Pre-requisites
#### Java 11
   ```sh
   cd /tmp  
   wget https://download.java.net/openjdk/jdk11/ri/openjdk-11+28_linux-x64_bin.tar.gz
   tar zxvf openjdk-11+28_linux-x64_bin.tar.gz
   mv jdk-11* /usr/local/
   #setting environment variables
   echo "export JAVA_HOME=/usr/local/jdk-11 \n export PATH=$PATH:$JAVA_HOME/bin" > /etc/profile.d/jdk.sh
   #source your file and check the java version
   source /etc/profile
   java -version
   ```
### Install Apache Tomcat
1. Download tomcat.
   ```sh 
   mkdir /opt/tomcat && cd /opt/tomcat
   wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.0.14/bin/apache-tomcat-10.0.14.tar.gz
   tar -xvzf /opt/apache-tomcat-apache-tomcat-10.0.14.tar.gz
   ```
2. Give executing permissions to startup.sh and shutdown.sh which are under bin. 
   ```sh
   chmod +x /opt/tomcat/apache-tomcat-10.0.14/bin/startup.sh 
   chmod +x /opt/tomcat/apache-tomcat-10.0.14/bin/shutdown.sh
   ```
3. Create link files for tomcat startup.sh and shutdown.sh.
   ```sh
   ln -s /opt/tomcat/apache-tomcat-10.0.14/bin/startup.sh /usr/local/bin/tomcatup
   ln -s /opt/tomcat/apache-tomcat-10.0.14/bin/shutdown.sh /usr/local/bin/tomcatdown
   tomcatup
   ```
   
    Access tomcat application from browser on port 8080  
   - http://<Public_IP>:8080

4. Now application is accessible on port 8090, but tomcat application doesn't allow to login from browser. Changing a default parameter in context.xml does address this issue.
   ```sh
   #search for context.xml
   find / -name context.xml
   ```
5. Above command gives 3 context.xml files. comment (<!-- & -->) `Value ClassName` field on files which are under webapp directory. After that restart tomcat services to effect these changes. 
   ```sh 
   /opt/tomcat/webapps/host-manager/META-INF/context.xml
   /opt/tomcat/webapps/manager/META-INF/context.xml
   
   # Restart tomcat services
   tomcatdown  
   tomcatup
   ```
6. Update users information in the tomcat-users.xml.
   ```sh
	<role rolename="manager-gui"/>
	<role rolename="manager-script"/>
	<role rolename="manager-jmx"/>
	<role rolename="manager-status"/>
	<user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
	<user username="user1" password="user1" roles="manager-script"/>
	<user username="user2" password="user2" roles="manager-gui"/>
   ```
7. Restart the service and try to login to the tomcat application from the browser.
