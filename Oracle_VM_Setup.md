# ################# Oracle Linux OS Prerequisites Setup #################

## 1. Download and Install – Oracle VM VirtualBox (Current Version: 7.1.0)
   - Oracle website: https://www.oracle.com/ae/virtualization/technologies/vm/downloads/virtualbox-downloads.html
   - For Windows: Download the `.exe` file and follow the installation wizard.
   - For Linux: Download the package for your OS and follow the instructions.
   
   Once installation is complete:
   - Open Oracle VirtualBox Manager.
   - Go to the `File` menu > Tools > Network Manager.
   - Create a Host-only Network and assign an IPv4 prefix, e.g., 192.168.56.xx.

## 2. Download Developer Day - Hands-on Database Application Development Image (Oracle Database 23c Free VirtualBox Appliance)
   - URL: https://www.oracle.com/database/technologies/databaseappdev-vm.html
   - This virtual machine contains:
     - Oracle Linux 8.8
     - Oracle Database 23c Free
     - Oracle REST Data Services 24.2.3
     - Oracle SQLcl 24.2.0
     - Oracle APEX 24.1

   Requirements:
   - At least 4GB RAM
   - At least 20GB of free disk space

## 3. Import the Oracle VM VirtualBox Appliance:
   - Open Oracle VM VirtualBox.
   - Go to the `File` menu > Open Import Virtual Appliance.
   - Select the downloaded file and click through the steps to complete the import.
   - Optional: In VM settings, activate a new network adapter (Host-only Adapter).
   
   Start the VM and connect using any SSH tool like MobaXTerm.

## 4. Login Credentials for the VM:
   - Username: `oracle`
   - Password: `oracle`

   After connecting, switch to root:
   ```bash
   sudo su -
   ```

## 5. Enable Telnet:
   - Edit the repo file:
   ```bash
   vi /etc/yum.repos.d/custom-oracle-linux.repo
   ```

   Add the following:
   ```ini
   
		[ol8_baseos_latest]
		name=Oracle Linux 8 BaseOS Latest ($basearch)
		baseurl=https://yum.oracle.com/repo/OracleLinux/OL8/baseos/latest/$basearch/
		gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
		gpgcheck=1
		enabled=1

		[ol8_appstream]
		name=Oracle Linux 8 Application Stream ($basearch)
		baseurl=https://yum.oracle.com/repo/OracleLinux/OL8/appstream/$basearch/
		gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
		gpgcheck=1
		enabled=1

		[ol8_UEKR7]
		name=Latest Unbreakable Enterprise Kernel Release 7 for Oracle Linux 8 ($basearch)
		baseurl=https://yum.oracle.com/repo/OracleLinux/OL8/UEKR7/$basearch/
		gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
		gpgcheck=1
		enabled=1
		
		[mongodb-org-6.0]
		name=MongoDB Repository
		baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/6.0/x86_64/
		gpgcheck=1
		enabled=1
		gpgkey=https://www.mongodb.org/static/pgp/server-6.0.asc
		sslverify=0
		
		




   ```

   Then, run the following commands:
   ```bash
   sudo dnf config-manager --set-disabled ol8_baseos_latest
   sudo dnf config-manager --set-disabled ol8_appstream
   sudo dnf clean all
   sudo dnf config-manager --set-disabled ol8_baseos_latest_static
   sudo dnf config-manager --set-disabled ol8_appstream_static
   sudo dnf repolist
   sudo dnf clean all
   sudo dnf update -y
   sudo dnf install telnet-server xinetd -y
   sudo yum install telnet -y
   ```

