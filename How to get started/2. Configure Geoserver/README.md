# Configure Geoserver

In this section:
- Configure the PostgreSQL database including installing PostGIS.
- Populate the database with some geospatial data
- Provision the GeoServer container.
- Create a load-balanced service for the Geoserver container.

## Create ECS Cluster
We will now create an ECS cluster to support Fargate container instances to host Geoserver application.

1. Type ECS into the search bar at the top of the screen and select Elastic Container Service from the search results.
2. On the ECS console select Clusters from the left hand menu then and click the Create Cluster button.
![Screenshot 2024-11-07 205006](https://github.com/user-attachments/assets/eb8d2ff6-9db5-4e9c-80b4-fb30c40f8f14)
3. On the Create Cluster screen enter the following then click Create Cluster
   - Cluster Name: HOL-Cluster
Leave Infrastructure settings as is to support Fargate based serverless deployment. Click Create to create the cluster.

This will take a few minutes to complete. Troubleshooting: If this operation fails with an error the first time rerun the create cluster instructions above.

![image](https://github.com/user-attachments/assets/df9c359d-9aa7-4d34-8853-19a86758ab8f)

## Run Geoserver
Now we have all the components in place we can run an instance of Geoserver.

1. On the task definition screen from the end of the last set you can Select Deploy and Run Task.
![Screenshot 2024-11-07 211018](https://github.com/user-attachments/assets/c151b21c-72d0-46ea-a67b-22367c463d5a)
2. Select your ECS cluster in the Existing Cluster field.
![Screenshot 2024-11-07 211048](https://github.com/user-attachments/assets/96b9dfc5-dbb6-439a-bb02-bc23d501237d)
3. Scroll down and open the Networking panel and expand it. Configure the following:
   - VPC: Select the Geoserver-VPC
   - Subnets: Make sure only the 3 subnets with public in their name are selected.
   - Security Group Name: Select the HOLGeosever security group. And remove the default security group
   - Public IP: make sure this is enabled.
![Screenshot 2024-11-07 211207](https://github.com/user-attachments/assets/79b02d7a-a58f-4422-bee1-44fa20be62d0)

4. Scroll to bottom and click Create to start the Geoserver container.
Once your task shows as running in the console click on the link in the Task column to view the task details.
![Screenshot 2024-11-07 211252](https://github.com/user-attachments/assets/0e98f6a3-b780-4ccc-ab3b-f02a54bbe6f5)

5. Locate the public IP address of your container. Copy and paste it into a text editor as we will use it shortly.
![Screenshot 2024-11-07 211406](https://github.com/user-attachments/assets/276e8d72-38c5-48e6-9c96-d5ab48bc6efc)
