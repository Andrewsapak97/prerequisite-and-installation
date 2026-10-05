<p align="center">
  <img src="[i.imgur.com](https://i.imgur.com/Clzj7Xs.png)" alt="osTicket logo" width="500">
</p>

# osTicket - Prerequisites and Installation

## Project Overview

This project documents how I installed osTicket, an open-source help-desk ticketing system, on a Windows virtual machine hosted in Microsoft Azure. I configured the web server, PHP environment, database server, application files, and permissions required to create a functioning osTicket installation.

This project demonstrates my ability to deploy cloud resources, administer a Windows environment, install supporting software, configure a web application, troubleshoot dependencies, and document a technical process.

## Project Objectives

- Deploy a Windows virtual machine in Microsoft Azure
- Connect to and administer the virtual machine through Remote Desktop
- Configure Internet Information Services to host osTicket
- Install and configure PHP and its required extensions
- Install MySQL and create an osTicket database
- Deploy the osTicket application files
- Complete and verify the web-based installation
- Secure the environment after installation

## Environments and Technologies Used

- Microsoft Azure Virtual Machines
- Windows 10 (21H2)
- Remote Desktop Protocol
- Internet Information Services (IIS)
- PHP
- PHP Manager for IIS
- MySQL Server
- HeidiSQL
- Microsoft Visual C++ Redistributable
- osTicket

## Prerequisites

Before beginning the installation, I needed:

- An active Microsoft Azure account
- A Windows virtual machine
- Remote Desktop access to the virtual machine
- Administrator access inside Windows
- Internet Information Services with CGI enabled
- PHP and PHP Manager for IIS
- MySQL Server
- HeidiSQL
- Microsoft Visual C++ Redistributable
- osTicket installation files

## Installation Steps

### 1. Create the Azure Virtual Machine

I created a resource group and deployed a Windows virtual machine in Microsoft Azure. I placed the resources in the same Azure region and selected a virtual machine configuration capable of running IIS, PHP, MySQL, and osTicket.

After the deployment was completed, I connected to the virtual machine through Remote Desktop. The remaining installation and configuration steps were performed inside this Windows environment.

![Azure virtual machine deployment](images/01-azure-vm.png)

*Figure 1: Windows virtual machine deployed in Microsoft Azure.*

### 2. Enable IIS and CGI

I opened **Turn Windows features on or off** and enabled Internet Information Services. Under the application development features, I enabled **CGI**, which allows IIS to process PHP requests through FastCGI.

The important Windows features included:

- Internet Information Services
- World Wide Web Services
- Application Development Features
- CGI
- Common HTTP Features
- IIS Management Console

![IIS and CGI enabled](images/02-iis-cgi.png)

*Figure 2: IIS and CGI enabled through Windows Features.*

I verified that IIS was working by opening the following address in the virtual machine's web browser:

```text
[localhost](http://localhost)
```

The default IIS page confirmed that the web server was installed and running.

![IIS default page](images/03-iis-default-page.png)

*Figure 3: Default IIS page confirming that the web server is operational.*

### 3. Install the Required Software

I installed the supporting components required by the osTicket environment:

- Microsoft Visual C++ Redistributable
- PHP
- PHP Manager for IIS
- MySQL Server
- HeidiSQL

I extracted the PHP files into the following directory:

```text
C:\PHP
```

I then opened PHP Manager in IIS and registered the PHP executable so IIS could process the application's PHP files.

![PHP registered with IIS](images/04-php-manager.png)

*Figure 4: PHP registered with IIS through PHP Manager.*

### 4. Verify the PHP Configuration

To verify that PHP was communicating with IIS correctly, I created the following test file:

```text
C:\inetpub\wwwroot\phpinfo.php
```

The file contained:

```php
<?php
phpinfo();
?>
```

I opened the test page in a browser using:

```text
[localhost](http://localhost/phpinfo.php)
```

The PHP information page confirmed that IIS could process PHP files successfully. I deleted the test file after verification because it displayed detailed server configuration information.

![PHP information page](images/05-php-verification.png)

*Figure 5: PHP operating successfully through IIS.*

### 5. Enable the Required PHP Extensions

I used PHP Manager to enable the extensions required by osTicket, including:

```text
php_imap.dll
php_intl.dll
php_opcache.dll
```

After enabling the extensions, I restarted IIS so the configuration changes would take effect.

![Required PHP extensions](images/06-php-extensions.png)

*Figure 6: Required PHP extensions enabled for osTicket.*

### 6. Install and Configure MySQL

I installed MySQL Server to provide the database used by osTicket. This database stores tickets, users, agents, departments, and application settings.

During installation, I created an administrative password and confirmed that the MySQL service was running. I then connected to the server through HeidiSQL and created a dedicated database named:

```text
osTicket
```

![osTicket database in HeidiSQL](images/07-osticket-database.png)

*Figure 7: Dedicated osTicket database created in HeidiSQL.*

> Database passwords and other credentials are intentionally excluded from this documentation.

### 7. Deploy the osTicket Application Files

I downloaded and extracted the osTicket installation package. I copied the contents of its `upload` folder into the IIS web directory and renamed the destination folder `osTicket`.

The application files were placed in:

```text
C:\inetpub\wwwroot\osTicket
```

![osTicket files in the IIS directory](images/08-osticket-files.png)

*Figure 8: osTicket application files deployed to the IIS web directory.*

### 8. Prepare the Configuration File

Inside the osTicket `include` directory, I renamed:

```text
ost-sampleconfig.php
```

to:

```text
ost-config.php
```

I temporarily adjusted the file permissions so the web installer could write the required configuration settings.

![osTicket configuration file](images/09-config-file.png)

*Figure 9: osTicket configuration file prepared for installation.*

### 9. Complete the Web Installer

I opened the osTicket setup page in the browser:

```text
[localhost](http://localhost/osTicket/setup)
```

I entered the help-desk information, administrator account details, database name, database username, and database password. I reviewed the information and submitted the form to complete the installation.

![osTicket setup page](images/10-setup-page.png)

*Figure 10: Web-based osTicket installation form.*

### 10. Secure and Verify the Installation

After the installation was completed, I performed the following checks and security steps:

- Confirmed that the osTicket interface loaded
- Opened the staff control panel
- Removed or disabled the `setup` directory
- Restricted permissions on `ost-config.php`
- Confirmed that osTicket could communicate with MySQL
- Verified that the administrator account could sign in

The staff control panel was available at:

```text
[localhost](http://localhost/osTicket/scp/login.php)
```

![Successful osTicket installation](images/11-installation-complete.png)

*Figure 11: Successfully installed osTicket staff interface.*

## Troubleshooting

### PHP Extensions Were Reported as Missing

I reviewed the active PHP configuration in PHP Manager, enabled the required extensions, restarted IIS, and refreshed the osTicket installer.

### osTicket Could Not Connect to MySQL

I confirmed that the MySQL service was running and checked the database name, username, password, and user permissions entered in the installer.

### The Configuration File Was Not Writable

I checked that the file had been renamed correctly to `ost-config.php`. I temporarily granted the access required by the installer and restricted the permissions after installation.

### The osTicket Page Would Not Load

I verified that IIS was running, confirmed that the application files were in the correct web directory, and checked that PHP had been registered correctly through PHP Manager.

## Security Considerations

During this project, I followed several basic security practices:

- Passwords and credentials were not included in the documentation
- The PHP information test file was removed after testing
- The osTicket setup directory was removed or disabled after installation
- Permissions on `ost-config.php` were restricted after setup
- Screenshots were reviewed for sensitive information before publication

## Skills Demonstrated

- Microsoft Azure resource deployment
- Windows virtual-machine administration
- Remote Desktop access
- IIS and FastCGI configuration
- PHP installation and validation
- MySQL database administration
- Web-application deployment
- File and folder permission management
- Technical troubleshooting
- Security awareness
- Technical documentation

## Final Outcome

I successfully deployed osTicket on a Windows virtual machine hosted in Microsoft Azure. The completed environment included a functioning IIS web server, PHP runtime, MySQL database, and osTicket staff interface.

This project provided the technical foundation for the next stage of my portfolio: configuring osTicket agents, departments, teams, roles, service-level agreements, and help topics.

## Author

**Andrew Sapak**

- [GitHub](https://github.com/Andrewsapak97)
- [LinkedIn](https://www.linkedin.com/in/andrew-sapak-0282522b4/)
