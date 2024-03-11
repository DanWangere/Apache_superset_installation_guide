# Guide for Apache-Superset Installation in Ubuntu

**Apache Superset** is a powerful open-source data exploration and visualization platform. With Superset, you can create interactive dashboards, explore data, and gain insights from various data sources. In this guide, we'll walk through the installation process on **Ubuntu**. 

Installing **Apache Superset** can be a bit challenging, especially for those new to the platform. However, fear not! This guide will walk you through the process step by step, making it much simpler. By the end, you'll be ready to create stunning visualizations and explore your data effortlessly.

Let's get started! 🚀

## OS DEPENDENCIES
When **installing Apache Superset on Ubuntu**, there are specific **operating system (OS) dependencies** that play a crucial role in ensuring a successful installation. Let's break it down:

1. **Why OS Dependencies Matter**:
   - **Superset**, being a complex data exploration and visualization platform, relies on various system-level components.
   - These dependencies provide essential libraries, tools, and services that Superset needs to function seamlessly.

2. **What Are These Dependencies**:
   - They include packages related to **Python**, **SSL**, **database connectors**, and more.
   - For example, **gcc**, **libffi**, **openssl**, and **Python development headers** are essential.

3. **How They Impact Installation**:
   - Without these dependencies, Superset might encounter errors or fail to operate correctly.
   - By installing them upfront, you ensure a smoother experience during the installation process.

Remember, these OS dependencies are like the building blocks that pave the way for a stable and functional Superset installation. For detailed instructions, refer to the [official documentation](https://superset.apache.org/docs/installation/installing-superset-from-pypi/) .

For **Ubuntu**, the following command will ensure that the required dependencies are installed:
```bash
sudo apt-get install build-essential libssl-dev libffi-dev python3-dev python3-pip libsasl2-dev libldap2-dev
```
After installing the necessary dependencies , run:
```bash
sudo apt-get update
```
The `sudo apt-get update` command serves an essential purpose when working with **Ubuntu** and other **Debian-based Linux distributions**.

1. **Updating Package Lists**:
   - When you run `sudo apt-get update`, it fetches the **latest version of the package list** from your system's software repositories.
   - This includes both the **official distribution repository** and any **third-party repositories** you may have configured.
   - Essentially, it checks for updates to the available packages and dependencies.

2. **Why It's Necessary**:
   - Keeping your package lists up to date is crucial for **security**, **stability**, and **compatibility**.
   - Without this step, your system might miss out on important updates.

Remember, running `sudo apt-get update` is the first step toward maintaining a healthy and well-functioning Ubuntu system.
### Create a Python Virtual Environment

```bash
pip install virtualenv 
sudo apt install python3-pip
sudo apt-get update
sudo apt install python3.8-venv
```
### Create and Activate a Virtual Environment

```bash
python3 -m venv superset
source superset/bin/activate
```
### Get the Latest Python Tools and Pip

```bash
pip install --upgrade setuptools pip
```
## Superset Installation and Initialization

### Install Superset
```bash
pip install Pillow
# installs the Pillow library, which is a Python Imaging Library (PIL) fork used for image processing.
pip install apache-superset
```
### Initialize the Database
```bash
superset db upgrade
# sets up the necessary database schema and tables for Superset to function properly.
export FLASK_APP=superset
# sets the FLASK_APP environment variable to the value “superset”.
# Environment variables are used by Flask (the web framework Superset is built on) to configure various aspects of the application.
openssl rand -base64 42
# generates a random base64-encoded string with 42 characters.
# this string is typically used as the SECRET_KEY for securing sessions and other sensitive data in Superset.
export SUPERSET_SECRET_KEY=
# replace the empty assignment with the actual secret key generated in the previous step.
pip install marshmallow-enum
# used for serialization and deserialization of enum fields.
```
## Create an Admin User in Apache Superset
```bash
export FLASK_APP=superset
superset fab create-admin
```
### Load some data to play with
```bash
superset load_examples
```
### Create default roles and permissions
```bash
superset init
```
## To start a development web server on port 8088, use -p to bind to another port
```bash
 superset run -p 8088 --with-threads --reload --debugger
 #The server will listen on port 8088. By specifying port 8088, the server will be accessible via http://localhost:8088 (or the appropriate IP address if accessed remotely).
```
Ports are like communication channels, and each application can use a specific port to send and receive data.
## Set up MYSQL
If it runs now set up MYSQL as a backend database engine 
```bash
#Install the following packages
sudo apt-get install pkg-config
pip install pillow
pip install wheel
sudo apt-get install python3-dev default-libmysqlclient-dev build-essential
pip install mysqlclient
pip install cx_oracle
pip install pymssql
pip install pyhive
pip install pyhive
pip install sqlalchemy-redshift
```
### Create a database and a user who has the same rights as the root.
To be able to log into mysql install the following packages.
```bash
sudo apt install mysql-client-core-8.0
sudo apt-get install mysql-server
sudo mysql -u root
use mysql;
```
If you have never assigned a root password for MySQL, the server does not require a password at all for connecting as root.
```bash
UPDATE user SET plugin="mysql_native_password" WHERE User='root';
ALTER USER 'root'@'localhost' IDENTIFIED BY '<password>’;
# OR
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY '<password>';
```
```bash
FLUSH PRIVILEGES;
```
When you set up a database system (such as MySQL or MariaDB), you create user accounts with specific privileges. These privileges determine what actions each user can perform within the database.
Privileges include permissions like SELECT, INSERT, UPDATE, DELETE, and more. They also control access to specific databases, tables, and other database objects.
The FLUSH PRIVILEGES command is used to reload the privilege tables in the database server.
When you create or modify user accounts, grant or revoke privileges, or make any changes related to user access, you need to execute FLUSH PRIVILEGES to ensure that the changes take effect immediately.
It essentially refreshes the internal cache of user privileges and ensures that the database server recognizes the updated permissions.

### Create a database
```bash
CREATE DATABASE <database name>;
```
You do not have to create the database if you have the connection for an existing database. It is the connection string for the existing database that you will use in your config file.
## Create a sudo user superuser
```bash
CREATE USER '<username>'@'localhost' IDENTIFIED WITH mysql_native_password BY '<password>';
GRANT ALL PRIVILEGES ON superset.* TO '<username>'@'localhost';
GRANT ALL PRIVILEGES ON mysql.* TO '<username>'@'localhost' with grant option;
GRANT ALL PRIVILEGES on *.* to '<username>'@'localhost';
GRANT ALL PRIVILEGES ON <database name>.* TO '<username>'@'localhost';
FLUSH PRIVILEGES;
exit
```
### log in again to mysql
```bash
mysql -u root OR mysql -u root -p
GRANT RELOAD ON *.* TO '<username>'@'localhost';
```
#### Change Directory to the configfile to sqlalchemy url
```bash
cd superset/lib/python3.8/site-packages/superset
```
#### Go to the config file
```bash
sudo nano config.py
```
Set up the URL 
```bash
SQLALCHEMY_DATABASE_URI ="mysql://<username>: '<password>'@localhost/<database name>"
```
## Initialize Superset
```bash
superset init
```

