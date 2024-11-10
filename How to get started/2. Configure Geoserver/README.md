# Configure GeoServer

In this section:
- Configure the PostgreSQL database including installing PostGIS.
- Populate the database with some geospatial data
- Provision the GeoServer container.
- Create a load-balanced service for the Geoserver container.

## Create ECS Cluster
We will now create an ECS cluster to support Fargate container instances to host the Geoserver application.

1. Type ECS into the search bar at the top of the screen and select Elastic Container Service from the search results.
2. On the ECS console select Clusters from the left-hand menu then and click the Create Cluster button.
![Screenshot 2024-11-07 205006](https://github.com/user-attachments/assets/eb8d2ff6-9db5-4e9c-80b4-fb30c40f8f14)
3. On the Create Cluster screen enter the following then click Create Cluster
   - Cluster Name: HOL-Cluster
Leave Infrastructure settings as is to support Fargate-based serverless deployment. Click Create to create the cluster.

This will take a few minutes to complete. Troubleshooting: If this operation fails with an error the first time rerun the create cluster instructions above.

![image](https://github.com/user-attachments/assets/df9c359d-9aa7-4d34-8853-19a86758ab8f)

## Run Geoserver
Now we have all the components in place we can run an instance of Geoserver.

1. On the task definition screen from the end of the last set you can Select Deploy and Run Task.
![Screenshot 2024-11-07 211018](https://github.com/user-attachments/assets/c151b21c-72d0-46ea-a67b-22367c463d5a)
2. Select your ECS cluster in the Existing Cluster field.
![Screenshot 2024-11-07 211048](https://github.com/user-attachments/assets/96b9dfc5-dbb6-439a-bb02-bc23d501237d)
3. Scroll down open the Networking panel and expand it. Configure the following:
   - VPC: Select the GeoServer-VPC
   - Subnets: Make sure only the 3 subnets with the public in their name are selected.
   - Security Group Name: Select the HOLGeosever security group. And remove the default security group
   - Public IP: make sure this is enabled.
![Screenshot 2024-11-07 211207](https://github.com/user-attachments/assets/79b02d7a-a58f-4422-bee1-44fa20be62d0)

4. Scroll to the bottom and click Create to start the GeoServer container.
Once your task shows as running in the console click on the link in the Task column to view the task details.
![Screenshot 2024-11-07 211252](https://github.com/user-attachments/assets/0e98f6a3-b780-4ccc-ab3b-f02a54bbe6f5)

5. Locate the public IP address of your container. Copy and paste it into a text editor as we will use it shortly.
![Screenshot 2024-11-07 211406](https://github.com/user-attachments/assets/276e8d72-38c5-48e6-9c96-d5ab48bc6efc)

## Use and configure Geoserver
1. Open a new browser tab and enter the following :
http://IP from last step]:8080/geoserver/web. NOTE: URL is case sensitive!

This should open the Geoserver home page.
![Screenshot 2024-11-07 211528](https://github.com/user-attachments/assets/99596d74-a0bb-4798-aacd-35491135c533)


![Screenshot 2024-11-07 211545](https://github.com/user-attachments/assets/59088e6f-1ffb-4bb0-a71c-810905be5526)
2. As this is the default configuration you can login with
   - Username: admin
   - Password: geoserver.
Make sure you change this when building Geoserver for your own purposes.
![Screenshot 2024-11-07 211636](https://github.com/user-attachments/assets/796eaf97-0ddf-4890-a804-20b30ea59526)
![Screenshot 2024-11-07 211734](https://github.com/user-attachments/assets/c2a79982-cfe9-4158-bcc0-cd478d7f7e83)
3. You can click on Layer Preview to view some of the built-in layers for demonstration.
## Create a Workspace
We will create a Workspace for some hands on lab exercises within Geoserver to leverage the Amazon EFS file store and the Amazon Aurora PostgreSQL PostGIS database.
1. Click on Workspaces on the left tab.
![image](https://github.com/user-attachments/assets/cd94215f-a079-49f0-a7bf-95a7145d7c44)
2. Then click Add new workspace.
3. Enter a name for the workspace (HOLworkspace) and a URI (http://www.myholworkspace.com ), then click Save.
![image](https://github.com/user-attachments/assets/f42071e4-d67a-468e-9835-bb9414c78f9b)

Now we add some data stores to illustrate accessing the connected file system and the backend database.

4. Click on the Stores link on the left panel.
![image](https://github.com/user-attachments/assets/0c8e52d1-3c69-4c22-ad62-55101d103de0)

5. Then click Add new Store.
![image](https://github.com/user-attachments/assets/376ed06c-620a-43cf-8dd5-f14b47de18de)

6. Click on Shapefile under Vector Data Sources.

7. Enter the following details:
   - Workspace: Select HOLworkspace
   - Data Source Name: HOLefsusstates
   - Description: US states from EFS
![image](https://github.com/user-attachments/assets/ee36e7a9-53a3-49ec-b7f8-fa4b5ca45dbc)
Then :
- Click the browse link next to the Shapefile Location field.
- Click the data/ link
- Click shapefiles/ link
- Click the states.shp link to select the States shapefile.
![image](https://github.com/user-attachments/assets/905cb705-0bdf-4c4c-ae3f-1cc90c538f28)
8. Then click Save. This takes you to the New Layer screen. Click the Publish link to publish this data source as a layer.
![image](https://github.com/user-attachments/assets/41d61c5e-9411-4507-ac10-1a3b43f265dd)
9. In the Edit Layer screen update the Name to HOL EFS US States and Title to HOL EFS US States so you can identify this layer from the built-in layers.

![image](https://github.com/user-attachments/assets/2254ffaa-1d78-433b-a7f8-55dacd38558d)

10. Scroll down the screen to the Bounding Boxes section. Click on the Compute from Native bounds link.
![image](https://github.com/user-attachments/assets/5c14ebbb-9815-4c34-b4f6-f36122b1ba60)
11. Then click Save and you will see layer has been added.
12. You can now go to the Layer Preview area ( select from left hand menu) and click on OpenLayers next to your new layer to see the Published layer. I am selecting US population
![Screenshot 2024-11-07 212955](https://github.com/user-attachments/assets/38be1c40-79c5-4ac3-a0ad-a97217e3255e)
Congratulations you have published geospatial data stored on a shared file system using Geoserver running in a Fargate hosted docker container.
## Publish a layer from the PostgreSQL database.
Now let's connect to the database we created earlier and publish a layer from data in the PostgreSQL database.

13. On the Geoserver home page click on the Stores link.
![image](https://github.com/user-attachments/assets/42816ebe-d14d-4fc2-a3c2-2ae1c6e56f04)
 
14. Click Add new Store. 
![Screenshot 2024-11-07 213116](https://github.com/user-attachments/assets/4f99ac82-421c-4aae-884d-158fd35d7be5)

15. Select PostGIS under Vector Data sources.
![Screenshot 2024-11-07 213121](https://github.com/user-attachments/assets/1f6c432c-9ccb-4f75-985b-b52b5f2433e1)

16. Enter the following then click Save
    - Workspace: HOLworkspace.
    - Name: NYC-postgis
    - Description: PostGIS workshop db
    - Host: Paste in the RDS endpoint you copied earlier in the workshop.
    - Port: 5432
    - Database: lab_gis
    - user: gis_admin
    - Password: enter the password you used when creating the lab_gis user.
![Screenshot 2024-11-07 213338](https://github.com/user-attachments/assets/3527417c-264a-40de-95e3-7e067df291ea)
17. When the new layer page displays click on the Publish link next to the nyc_census_blocks row.
![image](https://github.com/user-attachments/assets/18c33eb4-2c4e-44bf-bd9a-96256242a30d)
18. On the Edit Layer page enter a Name (HOL_nyc_census_blocks) and Description (HOL_nyc_census_blocks) to make the layer easy to locate.
![image](https://github.com/user-attachments/assets/ced75d18-ce43-43b2-9dd5-f7680eb42d08)
19. Scroll down to the bounding boxes sections and click on the compute from native bounds link to populate bounding boxes.
![image](https://github.com/user-attachments/assets/06f085e0-74be-48d2-b582-855fb3203ddf)
20. Click Save to complete the layer definition.
21. Navigate to the Layer Preview page and click on the Open Layers link for this layer to preview.
![image](https://github.com/user-attachments/assets/ba3c7207-e8e9-4bae-a6f4-3f06e6f2b9f5)
Congratulations you have published data stored in your backend PostgreSQL PostGIS database into Geoserver.

## Create Target Group

We will create a Target Group which will be needed when we create a Load Balancer in the next step.

A Target Group defines what resources the load balancer will send traffic to, in our case this will be the GeoServer containers. Since we need to configure a custom port number (8080) and some custom health check information we need to set this up before we dive in to create the Load Balancer.

1. Open the EC2 console by typing EC2 into the search bar at the top of the screen. Click the EC2 search result.

2. In the left hand menu locate the Load Balancing section and click Target Groups

3. Click the Create Target Group button.
4. On the Specify group details page enter the following:
   - Choose Target Type: IP addresses
   - Target Group Name: HOLGEOTG
   - Protocol: HTTP
   - Port :8080
   - VPC: Select the Geoserver-vpc
5. Scroll down to the Health Checks area and expand the advanced health check settings. Enter the following:
Health Check Path: /geoserver/web
Success Codes: 200,302
![Screenshot 2024-11-07 214706](https://github.com/user-attachments/assets/5d7a78d4-5170-4c52-92fd-3521e95da37c)
![Screenshot 2024-11-07 214711](https://github.com/user-attachments/assets/63ff02d0-4229-4250-becb-00d66928595f)


6. Click Next and then on next screen click Create Target Group.
![Screenshot 2024-11-07 214730](https://github.com/user-attachments/assets/feaa6e66-69cd-44d8-b199-2b0ad612b304)

7. Select the newly created Target Group and in Actions menu select Edit Target Group Attributes
![Screenshot 2024-11-07 214755](https://github.com/user-attachments/assets/6c4d023b-e730-4e73-aa1f-b0e49d4b0a2e)

8. In the Target selection configuration Enable stickiness and then click Save changes. This is needed as Geoserver needs sticky sessions to operate behind a load balancer.
![Screenshot 2024-11-07 214829](https://github.com/user-attachments/assets/549dce80-3f27-48d1-a855-cf451f74d8f5)
![Screenshot 2024-11-07 214901](https://github.com/user-attachments/assets/d881071c-8023-453f-9d6c-d26eabdc946f)

## Create Load Balancer
We will now create an Application Load Balancer (ALB) to use in the next step when we scale out to multiple Geoserver tasks running as a service.
1. In the left hand menu locate the Load Balancing section and click Load Balancers.
![Screenshot 2024-11-07 214945](https://github.com/user-attachments/assets/8b4c8420-1d8e-4050-83eb-cf4f6eebcbab)
2. Click Create load balancer.
3. Click Create under Application Load Balancer.
![Screenshot 2024-11-07 214951](https://github.com/user-attachments/assets/439eda23-8007-4e5e-ad90-444b9809cf62)
4. Enter a Name (HOLALB) for the load balancer.
![Screenshot 2024-11-07 215039](https://github.com/user-attachments/assets/bdc37039-38cf-43c0-83f7-1a7850fa79d2)
5. In the network mapping section select the geoserver-vpc that you created and select all 3 availability zones. Ensure that the public subnet in each availability zone is selected.
![Screenshot 2024-11-07 215125](https://github.com/user-attachments/assets/90633812-d9fb-4a0f-8e5d-a7c372dd8174)
6. In the Security Groups section select the security group called HOLALB. If the default security group is preselected then untick it to remove it.
7. In the Listener and routing section leave Listener set to port 80. In the Default, action drop down select the Target group you created in the last step: HOLGEOTG in the dropdown.
![Screenshot 2024-11-07 215245](https://github.com/user-attachments/assets/abe915db-64a9-49c7-b8f1-bbd6dc15c3bd)

9. Click Create Load Balancer at the bottom to create the load balancer.

## Create Load Balanced Geoserver Service
At present we have a single instance of a Geoserver container running on Fargate. To complete the workshop, we will now create a service to run multiple container tasks and expose them using a load balancer.

1. Type ECS into the search bar at the top of the screen and select Elastic Container Service from the search results.

2. On the ECS cluster list page click the name of your cluster (HOLCluster). Click the Services tab, then click on the Create button.
![Screenshot 2024-11-07 215728](https://github.com/user-attachments/assets/cffb4273-e2b6-4ee2-ac1b-9376fc355a8d)

3. Scroll down to the Deployment Configuration section. Enter the following:
   - Family: Select the HOLgeoserver task definition
   - Service Name: HOLgeoserverservice
   - Desired tasks: 2
![Screenshot 2024-11-07 215837](https://github.com/user-attachments/assets/0296b58e-b60e-4e0a-8a53-65be54fcb194)
4. Scroll down to the Networking section and expand it. Enter the following:
   - VPC: Select the GeoServer-vpc
   - Subnets: Ensure only the public subnets are selected
   - Security Group Name: Select the HOLgeoserver security group. If the default security group is selected then de-select it.
![Screenshot 2024-11-07 215948](https://github.com/user-attachments/assets/abf0cd97-2e73-4107-ae33-d0c602667ada)
![Screenshot 2024-11-07 220006](https://github.com/user-attachments/assets/c600f261-bd25-41cb-a400-a9aa5f488822)
5. Expand the Load balancing section. Enter the following:
   - Load Balancer Type: Application Load Balancer
   - Application Load Balancer: select Use an existing Load Balancer
   - Load Balancer: select HOLALB
   - Listener: Select Use an existing listener then select 80:HTTP from the drop down.
   - Target Group: Select Use an existing Target Group then select HOLGEOTG from the target group name drop down.
   - Health Check Grace Period: 300
![Screenshot 2024-11-07 220213](https://github.com/user-attachments/assets/b18e5a74-f533-4dec-b883-39a511276919)
6. Scroll down to the Service Auto scaling area. Select Use service auto-scaling and enter the following:
   - Minimum Number of tasks: 2
   - Maximum Number of Tasks: 9
   - Policy Name: HOLscalepolicy
   - ECS Service Metric: ECSServiceAverageCPUUtilization
   - Target Value: 70
   - Scale-Out cooldown period: 300
   - Scale-in cooldown period: 300
![Screenshot 2024-11-07 220428](https://github.com/user-attachments/assets/5bf5a4d5-96db-40cb-861e-b46004dcc4cb)
7. Scroll to the bottom and click Create.

The deployment will take a few minutes and you can optionally click on the view in CloudFormation button to see what resources are being built out.

8. To monitor the deployment click on the service name to see the service details, including deployment status. Once the service is running and targets are healthy click on the View load balancer button to open the Load Balancer console.
![Screenshot 2024-11-07 220810](https://github.com/user-attachments/assets/e00a4d09-3a7b-4578-b3b5-9442d1d511bc)

9. In the Load Balancer details copy the DNS name of the Load Balancer.
![Screenshot 2024-11-07 220906](https://github.com/user-attachments/assets/a08f8b6d-797e-41ff-9cbf-10fb39892f80)

10. To test the deployment and view Geoserver open a new browser window. Paste in the load balancer DNS URL and postpend /geoserver/web to the end. (NOTE URL is case sensitive)
![Screenshot 2024-11-07 220926](https://github.com/user-attachments/assets/8bdacf9a-3645-4098-a06a-f5ed14b41052)

### Geoserver should come up and you can log in with the same username/password as before and view the layers you created previously. Note the fact that the same configuration and layers comes up on these new containers illustrates how the use of the common EFS file system allows a scale out architecture to operate successfully.
![Screenshot 2024-11-07 220954](https://github.com/user-attachments/assets/22a63200-fdf5-411a-b39a-8da6a772633a)

### Congratulations you have now completed the lab and created a load-balanced, auto-scaling Geoserver deployment running on Fargate containers without having to manage your own server infrastructure.

To see how CloudWatch Logs is managing logs out of the GeoServer container instances open up the CloudWatch console. Then navigate to the /ecs/geoserver Cloudwatch log. Within the logs, you should find multiple entries in the log corresponding to each of the GeoServer tasks running. Having logs grouped per task will facilitate management as the number of tasks grows and shrinks.
