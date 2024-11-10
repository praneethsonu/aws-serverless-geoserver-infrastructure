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
1. Click on Workspaces on left tab.
![image](https://github.com/user-attachments/assets/cd94215f-a079-49f0-a7bf-95a7145d7c44)
2. Then click Add new workspace.
![image](https://github.com/user-attachments/assets/f42071e4-d67a-468e-9835-bb9414c78f9b)
3. Enter a name for the workspace (HOLworkspace) and a URI (http://www.myholworkspace.com ), then click Save.
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
