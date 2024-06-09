<h2 align="center"> CREATING AN RDS INSTANCE WITH A READ REPLICA IN A DIFFERENT AZ</h2>

The purpose of this project is to demonstrate the creation of a Relational Database Service (RDS) Instance running MySQL and creating a Read Replica for the database in a different Availability Zone (AZ). 

A read replica is a copy of a primary database that reflects changes to the primary database in near real-time. Read replicas are mainly used to reduce read traffic to the primary database to improve the performance and scalability of the primary database.

<h3 align="left"> SUMMARY</h3>

Setting up the above database and its read replica involves launching an RDS running MySQL. I also launched an EC2 Instance to allow access to the database and visualized the database using phpMyAdmin which I installed on the EC2 Instance. To test and validate that the setup works correctly, I created a table on the primary database and populated a few fields and records. I then switched to the replica and tried populating the table again but it threw an error, which is the expected result since the replica is read-only.

PhpMyAdmin is a free open-source web-based interface for managing databases such as MySQL and MariaDB. It is written in PHP.

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/architecture-diagram.png" height="50%" width="70%"/>
</p>
<h5 align="center"> Network Architecture</h5>

<h2 align="left"> Creating the VPC</h2>

Based on the network architecture, I created a VPC with a CIDR of 192.168.0.0/16.

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/vpc.png" height="65%" width="50%"/>
</p>

<h2 align="left"> Creating Internet Gateway</h2>

Next, I created the internet gateway which I attached to the vpc.

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/igw.png" height="40%" width="60%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/attaching-igw-to-vpc.png" height="40%" width="60%"/>
</p>

<h2 align="left"> Creating Subnets</h2>

Next, I created the two public subnets in different AZs, Subnet A in us-east-1a and Subnet B in us-east-1c. After creation, I edited the subnet settings to auto-assign IPv4 address to both public subnets.

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/public-subnet-A.png" height="35%" width="45%"/>
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/public-subnet-B.png" height="35%" width="45%"/>
</p>

Configuring Subnet Settings to Auto-Assign IPv4 Address to Subnets

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/edit-subnet-settings.png" height="60%" width="80%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/enable-auto-assign-ip.png" height="60%" width="80%"/>
</p>

Click the save button after enabling the auto-assign public IPv4 address

<h2 align="left"> Creating and Configuring Route Table</h2>

A default route is usually created after a vpc is created. I edited the name to "Main-Route" and edited the routing table to send all internet-bound traffic through the IGW. I also associated both subnets with the main route.

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/editing-route-table.png" height="45%" width="65%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/associating-subnets.png" height="45%" width="65%"/>
</p>

<h2 align="left"> Configuring Security Group</h2>

I configured the default security group created by the VPC to allow SSH and HTTP traffic from any IPv4 address. Also, after I create the EC2 instance, I'll need to allow MySQL/Aurora traffic from the EC2 private IP address. The reason why I allowed all SSH and HTTP traffic from the internet is because I'll terminate all instances and set-up immediately after the lab.

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/sg-configuration.png" height="60%" width="80%"/>
</p>


<h2 align="left"> Launching An RDS Instance</h2>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/db-creation-method.png" height="40%" width="60%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/engine-option.png" height="70%" width="50%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/template-n-availability.png" height="40%" width="60%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/setting.png" height="40%" width="60%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/instance-configuration.png" height="45%" width="65%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/connectivity-1.png" height="45%" width="65%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/connectivity-2.png" height="40%" width="60%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/connectivity-3.png" height="40%" width="60%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/db-creation-completed.png" height="40%" width="60%"/>
</p>


<h2 align="left"> Launching The EC2 Instance</h2>

The next step is to configure and launch an EC2 Instance in the same subnet and availability zone as the RDS. During the configuration, I installed Apache2 (HTTPd) and downloaded the phpMyAdmin tar file into the /var/www/html directory. After launching the EC2 Instance, I re-configured the security group to allow MySQL traffic from the EC2 Instance's private IP.

As part of configuring the EC2 Instance to be able to communicate with the database, I logged into the EC2 Instance using SSH, extracted the phpMyAdmin package, renamed it (phpMyAdmin-latest-all-languages) as phpMyAdmin (as a directory) after deleting the tar file, navigated into the phpMyAdmin directory, copied the sample configuration file (config.sample.inc.php) into an actual configuration file (config.inc.php) and edited it to use the endpoint of the database as localhost.

user-data

```commandline
#!/bin/bash
yum update -y
amazon-linux-extras install php7.2 -y
yum install httpd -y
cd /var/www/html
wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz
systemctl enable httpd
systemctl start httpd
```

phpMyAdmin configuration

```commandline
sudo -i
cd /var/www/html
tar xvf phpMyAdmin-latest-all-languages.tar.gz
rm -rf phpMyAdmin-latest-all-languages.tar.gz
mv phpMyAdmin-latest-all-languages phpMyAdmin
cd phpMyAdmin
cp config.sample.inc.php config.inc.php
```

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/ec2-ami.png" height="45%" width="65%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/instance-type.png" height="45%" width="65%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/network-settings.png" height="40%" width="60%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/user-data.png" height="35%" width="50%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/sg-re-configuration.png" height="60%" width="80%"/>
</p>
<h5 align="center"> SG Re-Configuration</h5>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/logging-into-ec2-instance.png" height="40%" width="60%"/>
</p>
<h5 align="center"> Logging into EC2 Instance for phpMyAdmin Configuration</h5>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/editing-localhost-php-config-file.png" height="60%" width="80%"/>
</p>
<h5 align="center"> Editing Localhost for phpMyAdmin Configuration File</h5>


<h2 align="left"> Logging into the Database Using phpMyAdmin</h2>

Now that phpMyAdmin which is linked to the primary database has been completely configured, using the EC2 Instance's public IP (http://107.22.5.148/phpMyAdmin) and the login credentials that I set-up during the database creation, I logged into the phpMyAdmin dashboard. I created a database, a table and populated a few rows and columns. This is possible since I am in the primary database, but this should not be possible when I log into the read-only secondary database.

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/phpmyadmin-login-page.png" height="60%" width="40%"/>
</p>
<h5 align="center"> phpMyAdmin Login Page</h5>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/db-creation-on-php.png" height="60%" width="80%"/>
</p>
<h5 align="center"> Creating a Database</h5>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/table-creation-php.png" height="60%" width="80%"/>
</p>
<h5 align="center"> Creating a Table on the Database</h5>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/field-title-creation.png" height="60%" width="80%"/>
</p>
<h5 align="center"> Creating Field Titles for the Table</h5>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/db-table-populated.png" height="60%" width="80%"/>
</p>
<h5 align="center"> Query Result After Populating Table</h5>


<h2 align="left"> Creating The Read Replica</h2>

The next step is to create the read replica, which is read-only. I used the same settings as the primary DB with changes only to the name. To test and validate that the whole setup works, I'll log in to the read replica database and try to add more records to the table. The failure of the database to execute this command is a prove that the setup is well-configured and working well. 

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/read-replica-init.png" height="60%" width="80%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/read-replica-settings.png" height="60%" width="80%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/read-replica-instance-config.png" height="70%" width="50%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/read-replica-connectivity.png" height="60%" width="80%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/changing-local-host-to-read-replica-endpoint.png" height="60%" width="80%"/>
</p>
<h5 align="center"> Switching to the Read Replica Database by Changing the Localhost Address to The Endpoint of the Read Replica</h5>


The endpoint of each of the databases can be found in the connectivity and security table of the database.


<h2 align="left"> Testing and Validation</h2>

I went to the phpMyAdmin dashboard and tried adding a new record to the table and it failed.

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/creating-new-row-read-replica.png" height="60%" width="80%"/>
</p>
<h5 align="center"> Creating a New Record/Row</h5>

<p align="center">
<img src="https://github.com/Topzdomain/Setting-Up-an-RDS-Instance-With-Read-Replica/blob/main/screenshots/error-message.png" height="50%" width="50%"/>
</p>
<h5 align="center"> Error message</h5>
