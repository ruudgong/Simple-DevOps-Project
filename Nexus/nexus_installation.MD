


# Installing Sonatype Nexus Repository OSS on CentOS 7

Sonatype Nexus is a popular repository manager used worldwide for most of the components, binaries, and build artifacts. It comes with support for the Java Virtual Machine (JVM) ecosystem, including Gradle, Ant, Maven, and Ivy.
Compatible with standard tools including Eclipse, IntelliJ, Hudson, Jenkins, Puppet, Chef, Docker, etc., Sonatype Nexus repo can manage dev components through delivery for the binaries containers, assemblies, and finished goods.


#### System requirements
* Minimum CPUs: 4, Recommended CPUs: 8+
* Minimum physical/RAM on the host 8GB

# 1. Pre-installation

```
[root@localhost ~]$ hostnamectl set-hostname nexus
```
```
[root@nexus ~]$ yum update -y
```
```
#Install Java
```
```
[root@nexus ~]$ yum -y install java-1.8.0-openjdk java-1.8.0-openjdk-devel
```
```
After installation is complete, check the java version to make sure you are all set to go to the next step of downloading the Repo.
```
```
[root@nexus ~]$ java -version
```

# 2. Download Nexus Repository Manager 3

#### Navigate to the opt directory:

```
[root@nexus ~]$ cd /opt
```
```
[root@nexus opt]$ wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz
```
```
[root@nexus opt]$ tar -xvzf latest-unix.tar.gz
```
#### You should see two directories, including nexus files and nexus data directory.

```
[root@nexus opt]$ ls -lh
```
```
[root@nexus opt] mv nexus-3.20.1-01 nexus
```
```
[root@nexus opt] mv sonatype-work nexusdata
```


# 3. Set User/Permissions and Configurations
#### Add a user for a nexus service.

```
[root@nexus opt] useradd --system --no-create-home nexus
```

#### Set the ownership for nexus files and nexus data.
```
[root@nexus opt] chown -R nexus:nexus /opt/nexus
```
```
[root@nexus opt] chown -R nexus:nexus /opt/nexusdata
```

#### Change Nexus configuration and set the custom data directory

#### Edit “nexus.vmoptions”.

```
[root@nexus opt] vim /opt/nexus/bin/nexus.vmoptions
```
#### Change the data directory.
```
-Xms2703m
-Xmx2703m
-XX:MaxDirectMemorySize=2703m
-XX:+UnlockDiagnosticVMOptions
-XX:+LogVMOutput
-XX:LogFile=../nexusdata/nexus3/log/jvm.log
-XX:-OmitStackTraceInFastThrow
-Djava.net.preferIPv4Stack=true
-Dkaraf.home=.
-Dkaraf.base=.
-Dkaraf.etc=etc/karaf
-Djava.util.logging.config.file=etc/karaf/java.util.logging.properties
-Dkaraf.data=../nexusdata/nexus3
-Dkaraf.log=../nexusdata/nexus3/log
-Djava.io.tmpdir=../nexusdata/nexus3/tmp
-Dkaraf.startLocalConsole=false
```


#### Change the user for the nexus service account.

#### Edit “nexus.rc” file.

[root@nexus opt] vim /opt/nexus/bin/nexus.rc
#### Uncomment “run_as_user” parameter and add new value.

```
run_as_user="nexus"
```


#### Configure the open file limit of the nexus user.
```
[root@nexus opt] vim /etc/security/limits.conf
```
#### Add the below values to the file.

```
nexus - nofile 65536
```


# 4. Set Nexus as a System Service

#### Create the Systemd service file in “/etc/systemd/system/”.

```
[root@nexus opt] vim /etc/systemd/system/nexus.service
```
#### Add the following to the file.

```
[Unit]
Description=Nexus Service
After=syslog.target network.target

[Service]
Type=forking
LimitNOFILE=65536
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
User=nexus
Group=nexus
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
```
[root@nexus opt] systemctl daemon-reload
```
```
[root@nexus opt] systemctl enable nexus.service
```
```
[root@nexus opt] systemctl start nexus.service
```
#### Head over to  your browser and complete the setup

