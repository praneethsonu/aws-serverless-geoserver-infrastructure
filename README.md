# Serverless GeoServer Infrastructure with PostgreSQL and PostGIS Integration

## Description:
Deploy and auto-scale GeoServer with PostgreSQL and the PostGIS extension on AWS. This project leverages containerized deployment using AWS Fargate, enabling you to focus on geospatial data management without the complexities of server maintenance.

## Architecture Overview:
At the completion of this lab, you will have created an autoscaling Fargate service hosting GeoServer containers. This provides a highly available load-balanced architecture without the overhead of having to manage your own servers or load-balancing infrastructure. Supporting this is a highly available shared file system for the GeoServer configuration and data directory. We also provide a highly available PostgreSQL PostGIS database as a geospatial data source.

The architecture is illustrated in the figure below.

<img width="4060" alt="GeoserverArchitecture" src="https://github.com/user-attachments/assets/019ae062-6317-42ce-a58f-0600118d2133">

Note when running GeoServer in a clustered configuration there are some considerations to keep in mind when updating the shared data configuration as a GeoServer caches the configuration in memory. This is explained in more detail here:

https://geoserver.geosolutionsgroup.com/edu/en/clustering/clustering/passive/passive.html



Now let's get started and start building this out from scratch on AWS.

## Features

- Set up infrastructure.
- Configure Geoserver.
- Install PostGIS and load data

## Getting Started

Follow the instructions in the [docs](docs/) directory to set up and deploy the infrastructure.

## License


