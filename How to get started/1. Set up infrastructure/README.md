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
![Screenshot 2024-11-07 194209](https://github.com/user-attachments/assets/86d4201d-4cdb-481a-a5be-032c9514c420)
![Screenshot 2024-11-07 194230](https://github.com/user-attachments/assets/3086f559-12bb-4cb3-9de0-cefc127300df)

## Geoserver security group
This will be the security group around the Geoserver application we will create latter in the lab. We will allow traffic only on port 8080 from either the Application Load balancer for final set up or from your laptop IP in the single instance testing and setup phase.
1. From the VPC console select Security Groups from the left hand menu. Once opened click the Create security group button in the top right hand corner.
![Screenshot 2024-11-07 194323](https://github.com/user-attachments/assets/4db72a3f-a3e1-4f77-b8d8-831573060e37)
2. Enter the following details:
   - Security Group Name: HOL-Geoserver
   - Description: HOLGeoserver
   - VPC: Click the dropdown in the VPC field then select the geoserver-vpc VPC.
   ![image](https://github.com/user-attachments/assets/8956e4a6-4e79-42c7-808e-29874c46d7e0)

3. In Inbound rules click Add rule
   - Select Custom TCP from the type drop down.
   - Enter 8080 in the Port Range field.
   - Select MyIP from the Source field. This will lock down the instance to only your desktop for access.
4. Click the Add Rule Button again.
   - Select Custom TCP from the type drop down.
   - Enter 8080 in the Port Range field.
   - Click in the search field next to Source and select the HOLALB security group.
5. Click the Create security group button at the bottom of the frame. Note down the security group Id as you will need this for the next security group setup.
   ![Screenshot 2024-11-07 194529](https://github.com/user-attachments/assets/a75539c6-5470-468d-a0b6-0de25fb89d8b)
## EFS security group
Now we create the security group to control traffic flowing to the shared file system (Amazon EFS). So in this case we want to allow NFS traffic requests coming from the Geoserver instances.
1. Again click the Create security group button in the top right-hand corner.
2. Enter the following details:
   - Security Group Name: HOL-EFS
   - Description: HOLEFS
   - VPC: Click the dropdown in the VPC field then select the geoserver-vpc VPC.
   ![Screenshot 2024-11-07 194641](https://github.com/user-attachments/assets/dc4ba123-bc3b-4096-9ed7-3d8854348fe7)
3. In Inbound rules click Add rule
   - Select NFS from the type drop down. This will set the port to 2049.
   - Click in the search field next to Source and select the HOLgeoserver security group. This will lock down the file system to only your Geoserver instances for access.
4. Click the Create security group button at the bottom of the frame. Note down the security group Id as you will need this for the next security group setup.
![Screenshot 2024-11-07 194723](https://github.com/user-attachments/assets/3ed85381-b312-4ccf-a7f3-c9aec819f251)
## PostGIS security group
Our final security group is to control traffic flowing to the backend database. Here we want to allow traffic from the Geoserver instances to the database.
1. Again click the Create security group button in the top right hand corner.
2. Enter the following details:
   - Security Group Name: HOLPostGIS
   - Description: HOLPostGIS
   - VPC: Click the dropdown in the VPC field then select the geoserver-vpc VPC.
3. In Inbound rules click Add rule
   - Select PostgreSQL from the type drop-down. This will set the port to 5432.
   - Click in the search field next to Source and select the HOLgeoserver security group. This will lock down the file system to only your Geoserver instances for access.
4. Click the Create security group button at the bottom of the frame. Note down the security group Id as you will need this for the next security group setup.
![Screenshot 2024-11-07 194935](https://github.com/user-attachments/assets/394e16f2-5915-4bbc-9a7d-4438320e9e19)







