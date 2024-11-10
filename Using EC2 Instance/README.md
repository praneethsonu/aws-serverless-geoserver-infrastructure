# Create admin EC2 instance to manage database

You can use these instructions if you wish to create an EC2 instance instead of using AWS CloudShell to run the command line configuration.

While we do not need to leverage EC2 instances to run our Geoserver infrastructure we will use an EC2 instance within the same VPC as the database administration client to upload some sample data.

1. Type EC2 into the search box at the top of the screen. Click the EC2 title from the search results.

2. On the EC2 console click the Launch Instance button.
![Screenshot 2024-11-07 221236](https://github.com/user-attachments/assets/7a5a70bc-7166-440f-ad9a-593366d840e7)
3. Name your instance and select an OS image instance size.
   - Name: myec2admin
   - Amazon Machine Image: Amazon Linux 2023 AMI (note this is the default image)
   - Instance Type: t2.micro (note this is the default instance type)
![image](https://github.com/user-attachments/assets/6f37049b-5b66-44ae-950f-9942cb06e0dd)

4. In the Key Pair (login) section:
   - Key Pair Name - required: Select "Proceed without a Key Pair"
5. In the Network Settings section click the Edit button and set the following:
   - VPC - required: select Geoserver-vpc
   - Subnet: Select one of the PUBLIC subnets. The subnet name will be similar to : geoserver-subnet-public*******
   - Auto assign Public IP: select Enable. (IMPORTANT Make sure you Enable assign Public IP)
   - Firewall (security Groups): click "Select existing security group"
   - Common Security Groups: select the HOLGeoserver group
![Screenshot 2024-11-07 221423](https://github.com/user-attachments/assets/bc7d38c6-af89-4971-8adf-c4b6f5f20c4b)

6. Expand the Advanced details section and next to IAM Instance Profile drop down click Create new IAM profile.
This will launch a separate browser tab to create an IAM role.
7. On the Roles screen click the Create Role button.
![Screenshot 2024-11-07 221504](https://github.com/user-attachments/assets/953fe8cc-0551-42dd-b651-30fdae3ce17e)
8. On the next screen make sure that AWS Service is selected in Trusted entity type. Select EC2 for "Service or use case" dropdown, select the EC2 Role for Systems Manager option, and then click Next.
![Screenshot 2024-11-07 221529](https://github.com/user-attachments/assets/519dd358-d5a0-439c-91c0-4ab4b1ff27f4)
9. In the Add Permissions step select the AmazonSSMManagedInstanceCore permission and click Next.
![Screenshot 2024-11-07 221619](https://github.com/user-attachments/assets/032b3cd8-7b3b-4b2f-ad92-e6d9c1d298de)

10. On the final screen enter a name for your role: HOLEC2SSMROLE then click Create Role.
![Screenshot 2024-11-07 221643](https://github.com/user-attachments/assets/9efc8091-3c27-46cd-8736-f641b118f535)
11. Now go back to the EC2 console tab and select the EC2 role name you just created in the IAM console above. Click the refresh button next to the dropdown if you don't see the name listed.
![Screenshot 2024-11-07 221720](https://github.com/user-attachments/assets/6960afc1-20e8-44ce-9b32-7e14cf9565e4)
12. Finally click the Launch Instance button in the Summary section.

## Verify your EC2 and RDS database are running
Before you move on verify that your EC2 instance and your RDS database are running.

13. Click the View all instances button to go back the EC2 console and list your EC2 instances.

14. In EC2 console check Instance State column shows Running and the Status Checks column shows 2/2 checks passed:
![Screenshot 2024-11-07 221903](https://github.com/user-attachments/assets/17d5fb57-a71c-454b-9881-a556992ad62d)

15. In RDS console check your database is available. To do this:
    - Type RDS in the top search bar then click RDS in the search results
    - From the RDS console dashboard select Databases from the left hand menu.
16. Click on the mypostgis name to open the database details. Click the radio button to the left of the mypostgis row where the Role column shows "Regional Cluster". (It should be pre-selected). In the Connectivity & Security tab locate the writer end point as shown below. Click the Copy icon to copy the end point address. Store this in a text editor as we will need it when connecting to the database.
![Screenshot 2024-11-07 221907](https://github.com/user-attachments/assets/79ebd046-b122-4e00-9617-78f3801bf53f)
![Screenshot 2024-11-07 222229](https://github.com/user-attachments/assets/1d8543a2-e709-404d-a719-1bc0b7cc5603)

## Connect to the EC2 instance (in Populate Database step)
1. Type EC2 into the search box at the top of the screen. Click the EC2 title from the search results.
2. Select Instances from the left hand menu then tick the box next to the EC2 instance that you just created. Then click the Connect button.
![Screenshot 2024-11-07 222236](https://github.com/user-attachments/assets/bce82965-fbe7-4454-89c3-789735e44437)

3. Select the Session Manager tab and click the Connect button.
![Screenshot 2024-11-07 222403](https://github.com/user-attachments/assets/2c1cb3b6-5545-4122-9cef-9aaf9ec84df5)

![Screenshot 2024-11-07 222408](https://github.com/user-attachments/assets/2b5861a7-0c4b-4374-94d1-fa6ff53c6f75)

This will open another browser tab and log you into the EC2 instance. Note we have been able to access the instance without opening port 22 in the security group or needing to use an ssh key.
