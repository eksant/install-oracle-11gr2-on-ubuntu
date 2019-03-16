# Install Oracle 11g R2 on Ubuntu

### Step 1: [Download](https://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/xe-prior-releases-5172097.html) Oracle Database Express Edition.

### Step 2: Instructions before install Oracle.
   a. Copy the downloaded file and paste it in home directory.  
   b. Unzip using the command:  
      ```
      unzip oracle-xe-11.2.0-1.0.x86_64.rpm.zip
      ```
   c. Install required packages using the command:  
      ```
      sudo apt-get install alien libaio1 unixodbc
      ```
   d. Enter into the Disk1 folder using command:  
      ```
      cd Disk1/
      ```
