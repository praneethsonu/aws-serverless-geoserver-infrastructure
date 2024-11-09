# Architecture Overview

This section outlines the architecture of the serverless GeoServer deployment on AWS.
<img width="4060" alt="GeoserverArchitecture" src="https://github.com/user-attachments/assets/6dd92206-e638-4ae0-856f-07c1333df714">
## Components

- **AWS Fargate**: Hosts GeoServer in containers.
- **Amazon RDS**: Manages PostgreSQL with PostGIS.
- **Amazon EFS**: Provides shared storage for GeoServer data.
