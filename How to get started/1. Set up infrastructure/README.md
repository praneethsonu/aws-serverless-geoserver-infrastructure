# Set up infrastructure

In this section:

- A Virtual Private Cloud (VPC).
- A PostgreSQL database using Amazon PostgreSQL.
- An Amazon Elastic File System (EFS) for shared storage.
- An administration server to allow you to perform database setup tasks.
- Security groups that will control access to the database and shared storage.

# Set up a Virtual Private Cloud (VPC)

1. Open the VPC console by typing VPC in the search bar at the top of the screen. Click the VPC title in the search results.
2. When the VPC dashboard loads click on Create VPC.
![Screenshot 2024-11-07 192032](https://github.com/user-attachments/assets/0646d81b-8b96-415f-ab20-509a94278ec7)
3. On the Create VPC screen do the following:
   - Select VPC and more in the Resources to Create section.
   - Enter geoserver for Name tag generation.
   - Select 3 for a number of availability zones.
   - Select 3 for a number of public and private subnets.
   - Select 1 for NAT gateways and none for VPC endpoints.
![Screenshot 2024-11-07 192315](https://github.com/user-attachments/assets/9b8b84bc-4e50-43d2-964b-5603a3116cda)

4. Click Create VPC.
   This will create and configure the VPC, subnets, route tables and Internet gateway. Once completed click the View VPC button to view what has been set up.

Note down the VPC ID of the VPC you have created as you will need this in the next step when you create the Security groups. To find the VPC ID you can see this in the VPC console by clicking on the Your VPCs link to see VPCs listed.

# Create security groups
Now that we have a Virtual Private Cloud, we need to create Security groups to allow appropriate traffic between the user, the Geoserver application, the shared file system and a backend PostgreSQL PostGIS database.

## Application Load Balancer Security Group
We will use an Application Load Balancer (ALB) to front-end our Geoserver service. We will create the security group for this now so we can reference to it in another security group.

1. From the VPC console select Security Groups from the left hand menu. Once opened click the Create security group button in the top right hand corner.
![Screenshot 2024-11-07 193958](https://github.com/user-attachments/assets/ad3365ca-2a03-43df-9435-d6b656716dbb)
2. Enter the following details:
   - Security Group Name: HOL-ALB
   - Description: HOL-ALB
   - VPC: Click the dropdown in the VPC field then select the geoserver-vpc VPC.
![Screenshot 2024-11-07 194108](https://github.com/user-attachments/assets/85951453-ee18-4808-9974-b211bacc9fbc)

3. In Inbound rules click Add rule
   - Select HTTP from the type drop down. This will select port 80
   - Select anywhere ipv4 0.0.0.0/0 from the Source field.
4. Click the Create security group button at bottom.
