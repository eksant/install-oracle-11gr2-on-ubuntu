# Install Oracle 11g R2 on Ubuntu

## Step 1: [Download](https://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/xe-prior-releases-5172097.html) Oracle Database Express Edition

## Step 2: Instructions before install Oracle

a. Copy the downloaded file and paste it in home directory.  
b. Install required packages using the command:

```bash
sudo apt install alien libaio1 unixodbc unzip
```

c. Unzip using the command:

```bash
unzip oracle-xe-11.2.0-1.0.x86_64.rpm.zip
```

d. Enter into the Disk1 folder using command:

```bash
cd Disk1/
```

e. Convert RPM package format to DEB package format (that is used by Ubuntu) using the command:

```bash
sudo alien --scripts -d oracle-xe-11.2.0-1.0.x86_64.rpm
```

f. Create the required chkconfig script using the command:

```bash
sudo nano /sbin/chkconfig
```

g. The pico text editor is started and the commands are shown at the bottom of the screen.  
Now copy and paste the following into the file and save:

```bash
#!/bin/bash
# Oracle 11gR2 XE installer chkconfig hack for Ubuntu
file=/etc/init.d/oracle-xe
if [[ ! `tail -n1 $file | grep INIT` ]]; then
   echo >> $file
   echo '### BEGIN INIT INFO' >> $file
   echo '# Provides: OracleXE' >> $file
   echo '# Required-Start: $remote_fs $syslog' >> $file
   echo '# Required-Stop: $remote_fs $syslog' >> $file
   echo '# Default-Start: 2 3 4 5' >> $file
   echo '# Default-Stop: 0 1 6' >> $file
   echo '# Short-Description: Oracle 11g Express Edition' >> $file
   echo '### END INIT INFO' >> $file
fi
update-rc.d oracle-xe defaults 80 01
```

h. Change the permission of the chkconfig file using the command:

```bash
sudo chmod 755 /sbin/chkconfig
```

i. Set kernel parameters. Oracle 11gR2 XE requires additional kernel parameters which you need to set using the command:

```bash
sudo nano /etc/sysctl.d/60-oracle.conf
```

j. Copy the following into the file and save:

```bash
# Oracle 11g XE kernel parameters
fs.file-max=6815744
net.ipv4.ip_local_port_range=9000 65000
kernel.sem=250 32000 100 128
kernel.shmmax=536870912
```

k. Verify the change using the command:

```bash
sudo cat /etc/sysctl.d/60-oracle.conf
```

l. You should see what you entered earlier. Now load the kernel parameters:

```bash
sudo service procps start
```

m. Verify the new parameters are loaded using:

```bash
sudo sysctl -q fs.file-max
```

You should see the file-max value that you entered earlier.

n. Set up /dev/shm mount point for Oracle. Create the following file using the command:

```bash
sudo nano /etc/rc2.d/S01shm_load
```

o. Copy the following into the file and save.

```bash
#!/bin/sh
case "$1" in
start)
    mkdir /var/lock/subsys 2>/dev/null
    touch /var/lock/subsys/listener
    rm /dev/shm 2>/dev/null
    mkdir /dev/shm 2>/dev/null
*)
    echo error
    exit 1
    ;;

esac
```

p. Change the permissions of the file using the command:

```bash
sudo chmod 755 /etc/rc2.d/S01shm_load
```

q. Now execute the following commands:

```bash
sudo ln -s /usr/bin/awk /bin/awk
sudo mkdir /var/lock/subsys
sudo touch /var/lock/subsys/listener
```

Now, Reboot Your System
