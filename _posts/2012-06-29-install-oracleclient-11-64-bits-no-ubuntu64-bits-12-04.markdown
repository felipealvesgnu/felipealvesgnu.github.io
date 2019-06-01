---
author: felipealvesgnu
comments: true
date: 2012-06-29 22:41:03+00:00
title: Install oracle client 11 (64 bits) on ubuntu(64 bits) 12
categories:
- Java
- Linux
tags:
- JBoss
- oracleclient
- Ubuntu
---

I have recently faced some difficulties in installing the oracle client in ubuntu and have JBoss 'see' the client installation, since it is approved for RedHat distributions, so I would like to share with you the settings that I, with the help of some friends of the work, I was able to make the connection with the UNICAMP - University of Campinas (where the project is located).

  
### Installing the JDK and setting JAVA_HOME

First follow the step-by-step [here](https://askubuntu.com/a/89080/68882).
After that set the environment variables in `/etc/profile` file adding the follow lines:

``` shell
$ export JAVA_HOME=/usr/lib/jvm/java-6-oracle
$ export PATH=$PATH:$JAVA_HOME/bin
```
#### PS.: Now restart the PC


## Getting started OracleClient 64 on ubuntu


### Step 1: Downloading the stuff

Go ahead and download Oracle Client 11g X64 (I'm using R1 cause I had it before, but R2 might work as well)

**Warning:** From now on, you should run all command as a root user. 
{: .notice--warning}

### Step 2: Updates

```shell
$ apt-get install alien build-essential gawk ksh lesstif2 
  libaio1 libaio-dev libtool expat unixodbc sysstat libxml2-dev 
  libxml2-utils zlib1g zlib1g-dev libjpeg8-dev libpng12-dev 
  libfreetype6-dev autoconf
```

### Step 3: Dash to Bash (as Dash is the default sh and we need Bash):
```shell
$ cd /bin
$ ln -sf bash /bin/sh
```

### Step 4: Creating user/group
```shell
 $ adduser oracle
```
Ubuntu will create an 'oracle' group aswell, just type 'oracle' as password and leave everything blank then ur done.

### Step 5: A Few Symlinks
```shell
$ cd /bin
$ ln -s /usr/bin/awk /bin/awk
$ ln -s /usr/bin/rpm /bin/rpm
$ ln -s /usr/bin/basename /bin/basename
$ mkdir /usr/lib64
$ ln -s /usr/lib/x86_64-linux-gnu/libpthread_nonshared.a /usr/lib64/libpthread_nonshared.a
$ ln -s /usr/lib/x86_64-linux-gnu/libc_nonshared.a /usr/lib64/libc_nonshared.a
```

### Step 6: Create Oracle Client's directory
```shell
$ mkdir -p /u01/app/
$ chown -R oracle:oracle /u01
$ xhost +
$ apt-get install synaptic
```

Open **synaptic**, to search **libstdc++ 5 **and install it, in some versions of Ubuntu is shipping a higher version.

### Step 7: Get ready to runInstaller

When you start the installation you'll choose the “Administrator option”, because it's a bit complete, even I need a client oracle oci connection. After that the oracle client installation will be displayed many errors, so you'll need to run the sh scripts.

```shell
$ su oracle
$ ./runInstaller
```

### Step 8: Update the file name databases

After to run the installation and its corrections run:
```shell
$ updatedb
```

### Step 9: Setting environment variables

**Info:** the oracle_home is the directory where the installation was performed, check in your environment...
{: .notice--info}

Add the environment variables in the `/etc/profile` file by adding the 2 lines below at the end of the file.
```bash
$ export ORACLE_HOME=/u01/app/oracle/product/11.2.0/client_1
$ export PATH=$PATH:$ORACLE_HOME/bin
``` 
  
### Step 10: Publish libs to Linux

You should register the oracle 11g lib path in `/etc/ld.so.conf.d`:
Create a file called `oracle.conf`,  inside `/etc/ld.so.conf.d` directory, with this content on the first line:
```bash
/u01/app/oracle/product/11.2.0/client_1/lib
```

Run the touch command:
```bash
$ touch /etc/ld.so.conf.d/oracle.conf
````

Finally
```bash
$ ldconfig -v   #Update the links/cache 
$ ldconfig -p | grep oracle 
``` 
the terminal may return an alert saying that it could not find the link for a given lib, normal this 'error' appears)


That's all :rocket: :rocket: :rocket:


