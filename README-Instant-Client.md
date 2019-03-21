# Install Oracle Instant Client on Ubuntu

To run ODPI-C applications with Oracle Instant Client zip files:

1. Download the latest Oracle Instant Client and SDK from the Oracle website zip file: [64-bit](http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html) or [32-bit](http://www.oracle.com/technetwork/topics/linuxsoft-082809.html), matching your application architecture.

   Look for `instantclient-basic-linux.x64-18.5.0.0.0dbru.zip` and `instantclient-sdk-linux.x64-18.5.0.0.0dbru.zip`.

2. Unzip the package into a single directory that is accessible to your application using command:

   ```bash
   mkdir -p /opt/oracle
   sudo mv -v instantclient-basic-linux.x64-18.5.0.0.0dbru.zip /opt/oracle
   sudo mv -v instantclient-sdk-linux.x64-18.5.0.0.0dbru.zip /opt/oracle
   cd /opt/oracle
   sudo unzip instantclient-basic-linux.x64-18.5.0.0.0dbru.zip
   sudo unzip instantclient-sdk-linux.x64-18.5.0.0.0dbru.zip
   ```

3. Install the libaio1 package with sudo or as the root user using command:

   ```bash
   sudo apt install libaio1
   ```

4. If there is no other Oracle software on the machine that will be impacted, permanently add Instant Client to the runtime link path using command, with sudo or as the root user:

   ```bash
   sudo sh -c "echo /opt/oracle/instantclient_18_5 > /etc/ld.so.conf.d/oracle-instantclient.conf"
   sudo ldconfig
   ```

   Alternatively:  
   set the environment variable LD_LIBRARY_PATH to the appropriate directory for the Instant Client version using command:

   ```bash
   export ORACLE_HOME=/opt/oracle/instantclient_18_5
   ```

   If you intend to co-locate optional Oracle configuration files such as tnsnames.ora, sqlnet.ora or oraaccess.xml with Instant Client, then create a network/admin subdirectory, if it does not exist using command:

   ```bash
   mkdir -p /opt/oracle/instantclient_12_2/network/admin
   ```

   This is the default Oracle configuration directory for applications linked with this Instant Client.

   Alternatively, Oracle configuration files can be put in another, accessible directory. Then set the environment variable TNS_ADMIN to that directory name.
