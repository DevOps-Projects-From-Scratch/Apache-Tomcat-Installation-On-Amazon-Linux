# Apache-Tomcat-Installation-On-Amazon-Linux
![Untitled](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/2db1c8be-b2d4-4c09-bb86-1d5a7253bc7c)

## Overview of Tomcat:
* Apache Tomcat, commonly referred to as Tomcat it is developed using the Java programming language.
* Apache Tomcat is an open-source web server and servlet container used for hosting Java web applications.
* It provides an environment to run Java Servlets and JavaServer Pages (JSPs) and is known for its simplicity and lightweight nature.

## Prerequisites:
* Apache Tomcat runs on port 8080 by default, so port 8080 must be open on EC2 Security Group.
* Java to be installed it can be JDK (Java Development Kit) or JRE (Java Runtime Environment) 
* Version of Java to be installed depends upon Tomcat version. 
* You can look for it here https://tomcat.apache.org/whichversion.html 

## RAM and hard disk requirements: 
* The RAM and hard disk requirements for Apache Tomcat can vary depending on the specific version of Tomcat, the size and complexity of the applications being deployed, and the expected workload.
### RAM Requirements:
* For basic testing or development purposes, a minimum of 1 GB of RAM is typically sufficient.
* For small-scale or low-traffic production environments, a minimum of 2-4 GB of RAM is recommended.
* For medium to large-scale deployments or high-traffic environments, you should consider allocating 8 GB or more of RAM. 
* The exact amount will depend on factors such as the number of concurrent users, the size and complexity of deployed applications, and any additional services running on the server.
### Hard Disk Requirements:
* The base installation of Apache Tomcat requires around 200-300 MB of disk space.
* Additional disk space will be needed for storing web applications, logs, and any other data generated by Tomcat.
* The amount of disk space required will depend on the size and number of deployed applications and the volume of log data generated. 

## Step-1: Launch an instance.
![1](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/53229dc5-f0c4-4f03-8ce8-fc643234f8de)

**Give a name to your instance:**
![2](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/2d52f842-354b-479f-8014-2b3e18a7a1aa)

**Select Amazon Linux-2 AMI:**
![3](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/40f6cafd-50bb-4530-a059-c51fedac0ceb)

**t2.micron is fine:**
![4](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/d2208c44-778f-40bd-be96-158dd8d098f2)

**I am going with existing keypair, if you dont have one create new one:**
![5](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/b37bb48c-4d74-4e48-8b25-660f0d65530e)

**Network setting:**
![6](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/0efa4447-417f-4bd0-aa9b-138d224aade2)

**Add new Inbound Security group rule to add 8080 port:**
![7](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/dd498c32-2176-457b-a6aa-4f048a2cbf07)

**8GB storage is fine:**
![8](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/6d8aa347-37db-4e6a-acf6-eaeb04b1b58b)

**Check the summary if everything is fine launch instance:**
![9](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/192698e1-3bbc-4922-93c9-d472f3d742c1)

**you can see instance running shortly:**
![10](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/5c9b0f96-8280-40a0-8dc3-c2a3c5826a03) 

## Step-2: Install Java.
* Connect to the instance and switch to root user.
* I am planning to install Tomcat-9 so Supported Java Versions are 8 or later. reffer https://tomcat.apache.org/whichversion.html 
```
amazon-linux-extras install java-openjdk11 -y
java --version
```
![11](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/80b6741b-ffb1-4be6-9e99-49d7b12cc50c)


## Step-3: Install Tomcat.
```
sudo su -
cd /
cd /opt
### Download tomcat binary
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.75/bin/apache-tomcat-9.0.75.zip

### unzip tomcat binary
unzip https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.75/bin/apache-tomcat-9.0.75.zip

### for convenience rename apache-tomcat-9.0.75 to tomcat.
mv apache-tomcat-9.0.75 tomcat
```
* Tomcat Binary files reffer page: https://tomcat.apache.org/download-90.cgi
![12](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/42adf6c7-615f-4def-8a5b-980bfdab5919)

![13](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/7cfea307-bc86-4302-983e-104e94fe2459)

![14](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/fb05de0e-cce3-4f06-b832-c3cd4371b9b6)


## Step-4: Add Execute Permission to catalina.sh, startup.sh & shutdown.sh
```
cd /opt/tomcat/bin
chmod +x catalina.sh startup.sh shutdown.sh
```
![15](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/12f78416-3fcc-4743-aeda-c75a1ba7669d)

## Step-5: Create link files for Tomcat Server up and Down (optional)
we create softlink to start and stop Tomcat service from anywhere.
if you Don't want to create softlink then you can start and stop Tomcat service by going to /opt/tomcat/bin directory.
```
ln -s /opt/tomcat/bin/startup.sh /usr/local/bin/tomcatup
ln -s /opt/tomcat/bin/shutdown.sh /usr/local/bin/tomcatdown
tomcatup 
```
![17](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/34d50d62-924f-4dbd-ae5d-969bfc645ec1)

### Before starting Tomcat we can't access tomcat by web-browser.
![16](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/9b9aea08-064a-4e36-81a2-88fe2b1790ad)

### After starting Tomcat we can access tomcat though web-browser.
![18](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/83597702-379a-415b-8e19-22026303aa58)

## Step-6: Update the user information in tomcat-users.xml

```
cd /opt/tomcat
cd conf

vi tomcat-users.xml

#Add below lines between <tomcat-users> tag

<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/>   
<user username="admin" password="admin" roles="manager-gui,manager-script,manager-jmx,manager-status"/>
<user username="deployer" password="deployer" roles="manager-script"/>
<user username="tomcat" password="tomcat" roles="manager-gui"/>

```
![23](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/4570ca18-d737-413a-98c6-d3ad7c7c3416)

## Step-7: Change Some Settings to Manage Tomcat
**To access the Tomcat Manager App, Server Status, Host Manager you need to comment out value tag in some xml file**
![19](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/94be6f6d-8eea-4a94-9f90-ec42ae46b20a)

**If you try to access Tomcat Manager App or Server Status or Host Manager with out doing the required settings then you will get 403 Access Denied Error.**
![20](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/d9915117-6a9f-4e63-894e-95751b6195bb)

```
cd /opt/tomcat
find -name context.xml
./conf/context.xml
./webapps/docs/META-INF/context.xml
./webapps/examples/META-INF/context.xml
./webapps/host-manager/META-INF/context.xml
./webapps/manager/META-INF/context.xml
#comment value tag sections in below all files
vi ./webapps/examples/META-INF/context.xml
vi ./webapps/host-manager/META-INF/context.xml
vi ./webapps/manager/META-INF/context.xml

```
**Before Commenting the Value tag**
![21](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/ea3be25b-9b85-4b9e-8574-79dfb37e4b29)

**After Commenting out the Value tag**
![22](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/e821cc96-ee6c-4909-99d0-0b897d38ca46)

**To Apply The Changes Stop and Start the Tomcat Service**
```
tomcatdown
tomcatup
```
![24](https://github.com/DevOps-Projects-From-Scratch/Apache-Tomcat-Installation-On-Amazon-Linux/assets/91256009/127ab08b-6ff9-43f8-b800-1c72ecf7eb59)



