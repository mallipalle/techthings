There are two versions of amazon linux.
1. Amazon Linux
2. Amazon Linux 2 (successor of first one)

We are going to use Amazon Linux 2.


### See which packages are available in your repos, just for info
```
yum search tomcat
```

### Install tomcat and tomcat-admin-webapps
tomcat-admin-webapp (usually called manager) is a separate package that you have to install along with tomcat if you need a nice GUI to manage tomcat and deploy apps via GUI.
```
sudo yum install tomcat tomcat-admin-webapps
```
Note 1: Amazon Linux 2 doesn't even come with java. Required java version will be installed as it is a dependency for tomcat.

Note 2: Tomcat is installed in **/usr/share/tomcat** folder.


### Make manager app accessible from everywhere
By default the manager webapp is restricted to the host in which tomcat is installed. Lets make it accessible from anywhere.
```
sudo nano /usr/share/tomcat/webapps/manager/META-INF/context.xml
```

Add the following line inside the `<context>` tag:
```
<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="^.*$" />
```

### Create the user: admin
By default no users are created for the tomcat. Lets create an admin user:
```
sudo nano /usr/share/tomcat/conf/tomcat-users.xml
```

Add the following lines inside `<tomcat-users>` tag:
```
<role rolename="admin"/> 
<role rolename="admin-gui"/> 
<role rolename="admin-script"/> 
<role rolename="manager"/> 
<role rolename="manager-gui"/> 
<role rolename="manager-script"/> 
<role rolename="manager-jmx"/> 
<role rolename="manager-status"/> 
<user name="admin" password="adminadmin" roles="admin,manager,admin-gui,admin-script,manager-gui,manager-script,manager-jmx,manager-status" />
```

### Open ports in firewall
In the security group of your ec2 instance make sure you open port 8080.

### To start Tomcat
```
sudo systemctl start tomcat.service
```

### check of the status of the tomcat service
```
sudo service tomcat status -l
```

### You can also check what ports are open and listening in your server
```
sudo netstat -ntlp | grep LISTEN
```
you can find 8080 listening

### Access the manager webUI
Go to 
```
<ec2-instance-public-IP>:8080/manager/html
```
Login with the credentials given when you created the admin user

### To stop tomcat
```
sudo systemctl stop tomcat.service
```
<br/>
<br/>
<br/>
<br/>

# Commands you may find useful in debugging issues related to installation

To check if java has been installed or to find out the version of java installed:
```
java -version
```

To see which packages are installed (for ex: to check if tomcat is installed or not):
```
rpm -qa|grep tomcat
```

To remove packages (for ex: remove all tomcat packages):
```
sudo yum remove tomcat*
```

To get the PID of tomcat process running (2nd column in the output)
```
ps -ef|grep tomcat
```

Get listening ports and search for this process ID
```
sudo netstat -nlp | grep <PID>
```

To find the command that started a process
```
ps aux |grep <PID>
```
OR
```
ps -p <PID> -o cmd
```

To find the directory from where a command is found in path or to find where is your command in the filesystem
```
whereis <yourcommand> (eg: `whereis java`)
```

