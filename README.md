# Install Oracle 11g R2 on Ubuntu

### Step 1: [Download](https://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/xe-prior-releases-5172097.html) Oracle Database Express Edition.

### Step 2: Instructions before install Oracle.
   a. Copy the downloaded file and paste it in home directory.  
   b. Install required packages using the command:  
      ```
      sudo apt install alien libaio1 unixodbc unzip
      ```  
   c. Unzip using the command:  
      ```
      unzip oracle-xe-11.2.0-1.0.x86_64.rpm.zip
      ```  
   d. Enter into the Disk1 folder using command:  
      ```
      cd Disk1/
      ```  
   e. Convert RPM package format to DEB package format (that is used by Ubuntu) using the command:  
      ```
      sudo alien --scripts -d oracle-xe-11.2.0-1.0.x86_64.rpm
      ```  
   f. Create the required chkconfig script using the command:  
      ```
      sudo nano /sbin/chkconfig
      ```  
   g. The pico text editor is started and the commands are shown at the bottom of the screen.  
      Now copy and paste the following into the file and save:  
      ```
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
