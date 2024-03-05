# Creating-EC2-Instances
Troubleshooting a Created EC2 Instances using AWS CLI 

I used the AWS Command Line Interface (AWS CLI) to launch Amazon Elastic Compute Cloud (Amazon EC2) instances using     these three software packages installed on a single machine which are often referred to as a LAMP stack (Linux,         Apache web server, MySQL, and PHP). Using a LAMP stack is a common way to create a website with a database backend     on a single machine.

The following diagram shows the architecture that I created in this activity.
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

Task 2: Configuring the AWS CLI
  I configured the AWS CLI by providing the configuration parameters that were made available when the lab was provisioned. After configuration, I run CLI commands to interact with AWS services.
  Note: Amazon Linux Amazon Machine Images (AMIs) already have the AWS CLI preinstalled.

  To set up the AWS CLI profile with credentials, run the following command:
        aws configure 
  When prompted, enter the following information:
      1. AWS Access Key ID: At the top of these instructions, choose Details, and then choose Show. Copy the AccessKey  
        value into the terminal window.
      2. AWS Secret Access Key: Copy the SecretKey value from the lab instructions into the terminal window.
      3. Default region name: Copy the LabRegion value from the lab instructions into the terminal window.
      4. Default output format: Enter json
  Now it was already connected to the CLI Host instance, I have configured and used the AWS CLI to call AWS services.
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/e028d340-9c2b-4d46-9c77-e0be68ea726b)

Task 3: Creating an EC2 instance by using the AWS CLI
In this task, I have observed and run a shell script, which was already provided, to create an EC2 LAMP instance by using AWS CLI commands. The script intentionally contains issues. The challenge is to find the issues and resolve them.

Task 3.1: Observe the script details
1. To change to the directory where the script file exists and create a backup of it, I run the following commands:
![attach](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/acef5731-6cb7-4b1e-83fe-b618c9b275e2)
Tip: It is a good practice to back up files before modifying them.
2. Open the create-lamp-instance-v2.sh script file in a command line text editor, such as VI.
Tip: To open the file in read-only mode in the VI editor, use the following command:
![attach2](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/75a6aa4e-d3eb-4a81-bf59-c1cb88eee58e)
view create-lamp-instance-v2.sh

Analyze the contents of the script:
Line 1: This is a bash file, so the first line contains #!/bin/bash.
![attached3](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/f5503bb7-669e-4b3a-b31a-1306c2461cb2)
Lines 7–11: The instance size is set to t3.small, which should be large enough to run the database and web server.
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/de5e52e5-4f79-429c-a949-f066be449233)
Lines 16–29: The script invokes the AWS CLI describe-regions command to get a list of all AWS Regions. In each Region, the script queries for an existing VPC that is named Cafe VPC. After finding the VPC, the script captures the VPC ID and Region, and breaks out of the while loop. This is the VPC where the LAMP instance for the Café Web Application should be deployed.
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/01981bf0-b1ac-4cb6-8c19-ae8cbc377770)
Lines 31–55:
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/80bea95f-d9b6-4270-b2a7-d4a8f76794e7)
The script invokes AWS CLI commands to look up the subnet ID, key pair name, and AMI ID values that are needed to create the EC2 instance.
Notice that line 32 ends with a backslash ( \ ) character. This character can be used to wrap a single command on another line in the script file. This technique is used many times in the script to make it easier to read.
Lines 57–122: The script cleans up the AWS account for situations where this script already ran in the account and is being run again. The script checks whether an instance that is named cafeserver already exists and whether a security group that includes cafeSG in its name already exists. If either resource is found, the script prompts you to delete them. 
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/cb648ac9-7773-4561-b98c-4e01e734936e)
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/7ab4e1f6-6fc2-43f6-aa81-ca97aa4f73c8)
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/b141b07d-7de5-4e4f-a1b4-0993bf23b3db)
Lines 124–152: The script creates a new security group with ports 22 and 80 open.
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/0830057d-33da-404b-a699-414df319ca50)
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/4d987608-4d32-4133-be68-1b29dfeef8f8)
Lines 154–168:
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/c311756d-e4d0-4519-a17c-50c9ecbd5b47)
The script creates a new EC2 instance. Notice how this part of the script uses the values that were set in lines 8 and 10, and the values that were collected in lines 16–57.
Notice the reference to the user data file. 
The entire call to create the instance is captured in a variable that is named instanceDetails. The contents of this variable are then echoed out to the terminal on line 177, and they are formatted for easier viewing by using a Python JSON tool.
Lines 179–188: The instanceId value is parsed out of the instanceDetails variable. Then, a while loop checks every 10 seconds to see if a public IP address has been assigned to the instance. After the check succeeds, the public IP address is written to the terminal.
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/9e8b06f6-f4bf-4333-9938-eab8d4d6a3f6)

To display the contents of the user data script 
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/0a620fc0-f378-4c88-824e-5b566a0e8486)
Notice how the user data script runs a series of commands on the instance after it is launched. These commands will install a web server, PHP, and a database server.
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/404d51be-532d-4067-9f3e-44c852437e85)
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/7637991f-861f-4215-bf94-8efe5ae73e40)


Task 3.2: Try to run the script
Now that I got an idea what the shell script is designed to do, I tried to run it:

Issue #1
The terminal output displays the following message: "An error occurred (InvalidAMIID.NotFound) when calling the RunInstances operation: The image id '[ami-xxxxxxxxxx]' does not exist".
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/343acfcd-5506-4c86-844d-be4d0fd9946b)
I have troubleshoot the issue by changing the Region in line 160, it was used us-east-1 but the Region used was us-west-2. After I fixed the issue, the run-instances command succeeded, and a public IPv4 address was assigned to the new instance. However, the script fails and exits without successfully completing. This behavior is expected.
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/77185d68-314c-480a-bfe1-7e37e5c9ced1)






Issue #1
The terminal output displays the following message: "An error occurred (InvalidAMIID.NotFound) when calling the RunInstances operation: The image id '[ami-xxxxxxxxxx]' does not exist".

Tips to help you resolve issue #1:

Locate the line in the script that led to this error. Does anything look incorrect in the script?
Was the correct Region value used for the run-instances command?
I changed the region 
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/4d4f3a65-120a-449a-aa87-5bbd719f3234)

After you identify the issue, update the script to fix the issue, and run the script again. If you are prompted to delete any existing security groups or instances that were created when the script ran previously, always reply with Y. Is the error resolved?
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/6364492a-93c9-4e8e-9c9b-6e893b33a6ef)

After you fix the issue, the run-instances command succeeds, and a public IPv4 address is assigned to the new instance.
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/f2bb46b2-73ff-48a9-a0e6-dcb8f4908ca9)





Try to connect to the webpage

In a browser, navigate to the following address. Replace <public-ip> with the Public IPv4 address of the new instance that you created: http://<public-ip>

The attempt fails. You need to resolve issue #2.


Tips to help you resolve issue #2:

The web server runs on TCP port 80. Is it open?
Can you verify that the web server service is running? The web server service name is httpd.


**i Added the port 80 for http and port 443 for https**
![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/2112c26c-a63e-43fd-bed2-a1711931c477)

![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/6f6e0b93-1875-494b-9837-8be3cc2b1bd8)




![image](https://github.com/VioletNavarro/Creating-EC2-Instances/assets/159421819/d5c997ec-fb84-4988-a3b9-c927df584cc4)


