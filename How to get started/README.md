# Set up infrastructure

In this section we will provision some of the key infrastructure components for your Geoserver solution. This will include:

A Virtual Private Cloud (VPC)
A PostgreSQL database using Amazon Aurora serverless
An Amazon Elastic File System (EFS) for shared storage
An administration server to allow you to perform database set up tasks.
Security groups which will control access to the database and shared storage.

# Configure Geoserver

In this section:

Configure the PostgreSQL database including installing PostGIS.
Populate the database with some geospatial data
Provision the Geoserver container
Create a load balanced service for the Geoserver container

