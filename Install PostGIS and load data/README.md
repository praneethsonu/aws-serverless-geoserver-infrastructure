# Install PostGIS and load data

1. If you have closed the CloudShell window; At the bottom of the AWS console browser window select CloudShell to open the AWS CloudShell command line.
2. This will open the CloudShell window.
3. Make sure you select the geoserverlab tab to use the within VPC command line environment you setup previously.
4. On the command line check your current directly and change to your home directory.
![Screenshot 2024-11-07 203430](https://github.com/user-attachments/assets/82848938-db85-485f-8f8c-2010c0f2f385)
5. (Note: This step not needed when using CloudShell CLI. Install only needed if you are using EC2 instance as in appendix) If postgres client tools not installed, install the local postgres client tools on the Instance by entering this command line:

```bash
sudo dnf install postgresql15 --assumeyes
```
### Set up PostGIS

Now the psql command line tool is installed we can connect to the database. You will need to know your database connection string which you can find in the RDS console for your database.

6. Retrieve the database end point address you copied earlier when setting up the database server. Copy and paste the following command into the terminal. Before you run it delete and past your RDS endpoint that you saved earlier. It should look something like: rdsendpoint=mypostgi.cluster-abcdefghijk.us-east-1.rds.amazonaws.com.
```bash
rdsendpoint=<RDS-endpoint>
```
Then copy and paste the following command to connect to your database.
```bash
psql --host=$rdsendpoint \
--port=5432 \
--username=pgadmin \
--password \
--dbname=firstdb
```
