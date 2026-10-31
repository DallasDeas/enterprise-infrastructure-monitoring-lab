# Getting Zabbix and its web interface working

[Walkthrough](../README.md) · [Full archive](../archive.md)

I deployed the monitoring stack in stages: database access, schema import, Zabbix service startup, and web frontend validation. The web interface failed in several distinct ways, including a 404, an Apache configuration error, and unprocessed PHP. I used those symptoms to narrow the configuration checks rather than treating a running process as proof of application health.

## 054: Checking the Zabbix package source

I refreshed package metadata and inspected the Zabbix server candidate. The official Zabbix 7.4 repository offered version 7.4.16 for this Ubuntu release.

![Screenshot 054: Checking the Zabbix package source](../screenshots/archive/screenshot_054.png)

## 055: Checking MariaDB

I confirmed MariaDB 11.8.6 was active and enabled. This was the database service that would support Zabbix.

![Screenshot 055: Checking MariaDB](../screenshots/archive/screenshot_055.png)

## 056: Creating the Zabbix database

I created the zabbix database with utf8mb4 and utf8mb4_bin, then listed databases to verify it existed.

![Screenshot 056: Creating the Zabbix database](../screenshots/archive/screenshot_056.png)

## 057: Checking database permissions

I granted the local Zabbix database user access to the Zabbix database and inspected its grants. The original output included a password hash, which is fully covered in this copy.

![Screenshot 057: Checking database permissions](../screenshots/archive/screenshot_057.png)

## 058: Testing database access

I logged in as the Zabbix database user and queried the current user and selected database. This checked access before moving on to the application schema.

![Screenshot 058: Testing database access](../screenshots/archive/screenshot_058.png)

## 059: Installing the Zabbix components

I checked the installed Zabbix server, frontend, Apache configuration, SQL scripts, and Agent 2 packages. The package list shows version 7.4.16.

![Screenshot 059: Installing the Zabbix components](../screenshots/archive/screenshot_059.png)

## 060: Locating the schema file

I confirmed the compressed server schema was present and checked MariaDB's binary-log settings before the import.

![Screenshot 060: Locating the schema file](../screenshots/archive/screenshot_060.png)

## 061: Validating the database schema import

I imported the Zabbix schema, the table structure required by the application, and counted 207 tables afterward. This verified that the database had been populated before I moved on to application startup.

![Screenshot 061: Validating the database schema import](../screenshots/archive/screenshot_061.png)

## 062: Starting the Zabbix server

The Zabbix server was active and its worker processes were running. It was still marked disabled in this capture, which I checked separately afterward.

![Screenshot 062: Starting the Zabbix server](../screenshots/archive/screenshot_062.png)

## 063: Reading the server log

I reviewed startup messages, unsupported optional items, and a local agent connection that failed and then recovered. I kept the log details instead of assuming every startup message was a fatal error.

![Screenshot 063: Reading the server log](../screenshots/archive/screenshot_063.png)

## 064: Checking startup behavior and Apache

I confirmed Zabbix was enabled and Apache was active and enabled. Apache also reported a server-name warning, which remained separate from whether it could serve the application.

![Screenshot 064: Checking startup behavior and Apache](../screenshots/archive/screenshot_064.png)

## 065: Hitting a 404 page

The first browser request to /zabbix returned Not Found. The server processes being up did not yet mean the frontend path was configured correctly.

![Screenshot 065: Hitting a 404 page](../screenshots/archive/screenshot_065.png)

## 066: Inspecting the Apache alias

I checked the Zabbix Apache configuration link and the /zabbix alias pointing at /usr/share/zabbix/ui.

![Screenshot 066: Inspecting the Apache alias](../screenshots/archive/screenshot_066.png)

## 067: Finding a configuration error

Apache's configuration test rejected ProxyTimeout. I used this error to focus on the web server configuration before trying another browser refresh.

![Screenshot 067: Finding a configuration error](../screenshots/archive/screenshot_067.png)

## 068: Getting a valid Apache configuration

The next configuration test returned Syntax OK. The fully qualified domain name warning was still present, so I did not confuse that warning with the earlier blocking error.

![Screenshot 068: Getting a valid Apache configuration](../screenshots/archive/screenshot_068.png)

## 069: Investigating unprocessed PHP

The browser displayed PHP source instead of the Zabbix interface. The request was reaching a file, but the server was not processing it as the application expected. I used that change in symptom to focus on Apache's PHP-FPM integration.

![Screenshot 069: Investigating unprocessed PHP](../screenshots/archive/screenshot_069.png)

## 070: Checking the PHP-FPM path

I enabled the Apache proxy-related modules, checked PHP 8.5 FPM, and reran the Apache configuration test. PHP-FPM was active and the configuration test passed.

![Screenshot 070: Checking the PHP-FPM path](../screenshots/archive/screenshot_070.png)

## 071: Enabling the FPM configuration

I enabled Apache's php8.5-fpm configuration and checked syntax again. This connected the web server configuration to the PHP runtime.

![Screenshot 071: Enabling the FPM configuration](../screenshots/archive/screenshot_071.png)

## 072: Confirming the handler

I checked that the FPM configuration was enabled and inspected the Unix-socket SetHandler entry used for PHP requests.

![Screenshot 072: Confirming the handler](../screenshots/archive/screenshot_072.png)

## 073: Verifying an HTML response

After starting Apache, I checked it was active and requested /zabbix locally with curl. The response contained HTML, and the next browser capture reached the installer.

![Screenshot 073: Verifying an HTML response](../screenshots/archive/screenshot_073.png)

## 074: Reaching the Zabbix installer

The Zabbix 7.4 welcome screen finally loaded in the browser. This was the visible result of the Apache and PHP troubleshooting.

![Screenshot 074: Reaching the Zabbix installer](../screenshots/archive/screenshot_074.png)

## 075: Checking frontend prerequisites

I reviewed the installer checks for PHP 8.5.4 and its required settings. The visible checks passed.

![Screenshot 075: Checking frontend prerequisites](../screenshots/archive/screenshot_075.png)

## 076: Entering the database connection

I reached the MySQL connection screen using localhost and the Zabbix database account. This intermediate capture has a blank password field; it is not a reusable credential configuration.

![Screenshot 076: Entering the database connection](../screenshots/archive/screenshot_076.png)

## 077: Reviewing frontend settings

I reached the frontend settings screen while its time zone still showed the default UTC selection.

![Screenshot 077: Reviewing frontend settings](../screenshots/archive/screenshot_077.png)

## 078: Setting the display time zone

I selected America/New_York for the Zabbix frontend. This matters when comparing its timestamps with Windows events later in the project.

![Screenshot 078: Setting the display time zone](../screenshots/archive/screenshot_078.png)

## 079: Reviewing the installation summary

I checked the database and frontend summary before completion. The screen also showed encryption was not configured for the database connection or web interface.

![Screenshot 079: Reviewing the installation summary](../screenshots/archive/screenshot_079.png)

## 080: Completing frontend installation

The installer reported that the Zabbix frontend had been installed successfully.

![Screenshot 080: Completing frontend installation](../screenshots/archive/screenshot_080.png)

## 081: Encountering a login error

An initial login attempt failed. The screenshot does not establish whether the cause was a mistyped credential or another login condition, so I do not assign a cause here.

![Screenshot 081: Encountering a login error](../screenshots/archive/screenshot_081.png)

## 082: Opening the working dashboard

I reached the dashboard with Zabbix server running and version 7.4.16 displayed. At this point it was monitoring itself; the Windows host came next. The map is the default dashboard widget, not a diagram of my lab.

![Screenshot 082: Opening the working dashboard](../screenshots/archive/screenshot_082.png)
