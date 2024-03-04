# Creating-EC2-Instances
Creation of an EC2 Instances using AWS CLI
I used the AWS Command Line Interface (AWS CLI) to launch Amazon Elastic Compute Cloud (Amazon EC2) instances using these three software packages installed on a single machine, they are often referred to as a LAMP stack (Linux, Apache web server, MySQL, and PHP). Using a LAMP stack is a common way to create a website with a database backend on a single machine.
  1. A user data script to configure the instance to have an Apache web server,
  2. A MariaDB relational database (which is a fork of the MySQL relational database),
  3. PHP running on the instance.  

The same user data file will deploy website files and run database configuration scripts on the instance. The result will be an instance that hosts the Café Web Application. 

The following diagram shows the architecture that you will create in this activity.
<img width="1568" alt="Architecture" src="https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/f7472561-9a45-49f1-9567-a0ba96ea6981">
