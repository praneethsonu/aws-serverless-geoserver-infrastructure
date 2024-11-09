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
