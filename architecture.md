# Architecture Overview

This section outlines the architecture of the serverless GeoServer deployment on AWS.

<img width="4061" alt="Geoserver Architecture" src="https://github.com/user-attachments/assets/c13b39ad-578d-46b0-84f6-88c364517a2a">

## Components

- **AWS Fargate**: Hosts GeoServer in containers.
- **Amazon RDS**: Manages PostgreSQL with PostGIS.
- **Amazon EFS**: Provides shared storage for GeoServer data.
