# 1. Set up infrastructure

In this section:

- A Virtual Private Cloud (VPC) and create Security Groups.
- A PostgreSQL database using Amazon RDS.
- An Amazon Elastic File System (EFS) for shared storage.
- An administration server to allow you to perform database setup tasks.
- Security groups that will control access to the database and shared storage.

# 2. Configure Geoserver

In this section:

- Configure the PostgreSQL database including installing PostGIS.
- Populate the database with some geospatial data
- Provision the GeoServer container.
- Create a load-balanced service for the Geoserver container.

