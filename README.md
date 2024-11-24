# AWS Serverless GeoServer Infrastructure with PostgreSQL and PostGIS Integration

## Description
Deploy and auto-scale GeoServer with PostgreSQL and the PostGIS extension on AWS. This project leverages containerized deployment using AWS Fargate, enabling you to focus on geospatial data management without the complexities of server maintenance.

## Architecture Overview

The architecture is illustrated in the figure below.

<img width="4060" alt="GeoserverArchitecture" src="https://github.com/user-attachments/assets/019ae062-6317-42ce-a58f-0600118d2133">

- **GeoServer**: An open-source server for sharing geospatial data.
- **AWS Fargate**: A serverless compute engine for containers.
- **PostgreSQL/PostGIS**: A spatial database extender for PostgreSQL.

Note: when running GeoServer in a clustered configuration there are some considerations to keep in mind when updating the shared data configuration as a GeoServer caches the configuration in memory. This is explained in more detail here:

https://geoserver.geosolutionsgroup.com/edu/en/clustering/clustering/passive/passive.html



Now let's get started and start building this out from scratch on AWS.
  
## Features

- Set up infrastructure.
- Configure Geoserver.
- Install PostGIS and load data

## Getting Started

Follow the instructions in the [How to get started](https://github.com/praneethsonu/aws-serverless-geoserver-infrastructure/tree/main/How%20to%20get%20started) directory to set up and deploy the infrastructure.

Also Check out my [AWS Community Post](https://community.aws/content/2p81D68CLjdrJs0Hq0FbqVM7yVN)

## License

This project is licensed under the MIT License.
