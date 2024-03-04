# Creating-EC2-Instances
Creation of an EC2 Instances using AWS CLI
I used the AWS Command Line Interface (AWS CLI) to launch Amazon Elastic Compute Cloud (Amazon EC2) instances using these three software packages installed on a single machine, they are often referred to as a LAMP stack (Linux, Apache web server, MySQL, and PHP). Using a LAMP stack is a common way to create a website with a database backend on a single machine.
  1. A user data script to configure the instance to have an Apache web server,
  2. A MariaDB relational database (which is a fork of the MySQL relational database),
  3. PHP running on the instance.  

The same user data file will deploy website files and run database configuration scripts on the instance. The result will be an instance that hosts the Café Web Application. 

The following diagram shows the architecture that you will create in this activity.
<img width="1568" alt="Architecture" src="https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/f7472561-9a45-49f1-9567-a0ba96ea6981">

Objectives
  - Launch an EC2 instance by using the AWS CLI.
  - Troubleshoot AWS CLI commands and Amazon EC2 service settings by using basic troubleshooting tips and the open-          source nmap utility.

Task 1: Connecting to the CLI Host instance
  I used EC2 Instance Connect to connect to the CLI Host EC2 instance that was created when the lab was provisioned. I     used this instance to run CLI commands.

    1. On the AWS Management Console, in the Search bar, enter and choose EC2 to open the Amazon EC2 Management Console.

    2. In the navigation pane, choose Instances.

    3. From the list of instances, select the  CLI Host instance.

    4. Above the list of instances, choose Connect.

    5. On the EC2 Instance Connect tab, choose Connect.
  
  Note: If you prefer to use an SSH client to connect to the EC2 instance, see the guidance to Connect to Your Linux     
  Instance.

Task 2: Configuring the AWS CLI
  In this task, you configure the AWS CLI by providing the configuration parameters that were made available to you when   the lab was provisioned. After configuration, you run CLI commands to interact with AWS services.

  Note: Amazon Linux Amazon Machine Images (AMIs) already have the AWS CLI preinstalled.

  To set up the AWS CLI profile with credentials, run the following command:
        aws configure

  Tip: In EC2 Instance Connect, you might need to use the context (right-click) menu to paste values in the terminal. 

  When prompted, enter the following information:
      1. AWS Access Key ID: At the top of these instructions, choose Details, and then choose Show. Copy the AccessKey   
        value into the terminal window.
      2. AWS Secret Access Key: Copy the SecretKey value from the lab instructions into the terminal window.
      3. Default region name: Copy the LabRegion value from the lab instructions into the terminal window.
      4. Default output format: Enter json
  Now that you are connected to the CLI Host instance, you can configure and use the AWS CLI to call AWS services.
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/e028d340-9c2b-4d46-9c77-e0be68ea726b)



