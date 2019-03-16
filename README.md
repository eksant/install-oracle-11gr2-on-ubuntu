# Install Oracle 11g R2 on Ubuntu

## Step 1: [Download](https://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/xe-prior-releases-5172097.html) Oracle Database Express Edition

## Step 2: Instructions before install Oracle

1. Copy the downloaded file and paste it in home directory.
2. Install required packages using the command:

   ```bash
   sudo apt install alien libaio1 unixodbc unzip
   ```

3. Unzip using the command:

   ```bash
   unzip oracle-xe-11.2.0-1.0.x86_64.rpm.zip
   ```

4. Enter into the Disk1 folder using command:

   ```bash
   cd Disk1/
   ```

5. Convert RPM package format to DEB package format (that is used by Ubuntu) using the command:

   ```bash
   sudo alien --scripts -d oracle-xe-11.2.0-1.0.x86_64.rpm
   ```

6. Create the required chkconfig script using the command:

   ```bash
   sudo nano /sbin/chkconfig
   ```

7. The pico text editor is started and the commands are shown at the bottom of the screen.  
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

8. Change the permission of the chkconfig file using the command:

   ```bash
   sudo chmod 755 /sbin/chkconfig
   ```

9. Set kernel parameters. Oracle 11gR2 XE requires additional kernel parameters which you need to set using the command:

   ```bash
   sudo nano /etc/sysctl.d/60-oracle.conf
   ```

10. Copy the following into the file and save:

    ```bash
    # Oracle 11g XE kernel parameters
    fs.file-max=6815744
    net.ipv4.ip_local_port_range=9000 65000
    kernel.sem=250 32000 100 128
    kernel.shmmax=536870912
    ```

11. Verify the change using the command:

    ```bash
    sudo cat /etc/sysctl.d/60-oracle.conf
    ```

12. You should see what you entered earlier. Now load the kernel parameters:

    ```bash
    sudo service procps start
    ```

13. Verify the new parameters are loaded using:

    ```bash
    sudo sysctl -q fs.file-max
    ```

    You should see the file-max value that you entered earlier.

14. Set up /dev/shm mount point for Oracle. Create the following file using the command:

    ```bash
    sudo nano /etc/rc2.d/S01shm_load
    ```

15. Copy the following into the file and save.

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

16. Change the permissions of the file using the command:

    ```bash
    sudo chmod 755 /etc/rc2.d/S01shm_load
    ```

17. Now execute the following commands:

    ```bash
    sudo ln -s /usr/bin/awk /bin/awk
    sudo mkdir /var/lock/subsys
    sudo touch /var/lock/subsys/listener
    ```

    Now, Reboot Your System

## Step 3: Install Oracle

1. Install the oracle DBMS using the command:

   ```bash
   sudo dpkg --install oracle-xe_11.2.0-2_amd64.deb
   ```

2. Configure Oracle using the command:

   ```bash
   sudo /etc/init.d/oracle-xe configure
   ```

3. Setup environment variables by editting your .bashrc file:

   ```bash
   sudo nano ~/.bashrc
   ```

4. Add the following lines to the end of the file:

   ```bash
   export ORACLE_HOME=/u01/app/oracle/product/11.2.0/xe
   export ORACLE_SID=XE
   export NLS_LANG=`$ORACLE_HOME/bin/nls_lang.sh`
   export ORACLE_BASE=/u01/app/oracle
   export LD_LIBRARY_PATH=$ORACLE_HOME/lib:$LD_LIBRARY_PATH
   export PATH=$ORACLE_HOME/bin:$PATH
   ```

5. Load the changes by executing your profile:

   ```bash
   . ~/.bashrc
   ```

6. Start the Oracle 11gR2 XE:

   ```bash
   sudo service oracle-xe start
   ```

7. Add user YOURUSERNAME to group dba using the command:

   ```bash
   sudo usermod -a -G dba YOURUSERNAME
   ```

## Step 4: Using the Oracle XE Command Shell

1. Start the Oracle XE 11gR2 server using the command:

   ```bash
   sudo service oracle-xe start
   ```

2. Start command line shell as the system admin using the command:

   ```bash
   sqlplus sys as sysdba
   ```

   Enter the password that you gave while configuring Oracle earlier. You will now be placed in a SQL environment that only understands SQL commands.

3. Create a regular user account in Oracle using the SQL command:

   ```bash
   create user USERNAME identified by PASSWORD;
   ```

   Replace USERNAME and PASSWORD with the username and password of your choice. Please remember this username and password. If you had error executing the above with a message about resetlogs, then execute the following SQL command and try again:

   ```bash
   alter database open resetlogs;
   ```

4. Grant privileges to the user account using the SQL command:

   ```bash
   grant connect, resource to USERNAME;
   ```

   Replace USERNAME and PASSWORD with the username and password of your choice. Please remember this username and password.

5. Exit the sys admin shell using the SQL command:

   ```bash
   exit;
   ```

6. Start the commandline shell as a regular user using the command:

   ```bash
   sqlplus
   ```
