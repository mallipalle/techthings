There are two versions of amazon linux.
1. Amazon Linux
2. Amazon Linux 2 (successor of first one)

We are going to use Amazon Linux 2.
Amazon Linux 2 doesn't even come with java.



To install tomcat:

```
sudo yum install tomcat tomcat-admin-webapps
```
(required java version will be installed as it is a dependency for tomcat)





###Command you may find useful

To check if java has been installed or to find out the version of java installed:

```
java -version
```

To search the repositories for packages:
`yum search tomcat`

To see which packages are installed:
`rpm -qa|grep tomcat`

To remove packages (for ex: tomcat):
`sudo yum remove tomcat*`
