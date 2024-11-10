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
This will prompt you for the pgadmin password that you entered when creating the database and then put you in the psql command line.
![Screenshot 2024-11-07 203635](https://github.com/user-attachments/assets/985678a5-1b0f-4844-9b2b-427356936d66)

7. Create a separate role (user) to administer the PostGIS extension.
```bash
CREATE ROLE gis_admin LOGIN PASSWORD 'change_me';
```
8. Grant this role rds_superuser privileges, to allow the role to install the extension.
```bash
GRANT rds_superuser TO gis_admin;
```
9. Create a database to use for your PostGIS artifacts.
```bash
CREATE DATABASE lab_gis;
```
10. Give the gis_admin all privileges on the lab_gis database.
```bash
GRANT ALL PRIVILEGES ON DATABASE lab_gis TO gis_admin;
```
11. Exit the session (use quit) and reconnect to your RDS for PostgreSQL DB instance as gis_admin.
```bash
quit
```
```bash
psql --host=$rdsendpoint \
    --port=5432 \
    --username=gis_admin \
    --password \
    --dbname=lab_gis
```
Once you enter the gis_admin password it will again take you to the psql command line. Note the lab_gis prompt this time.
![Screenshot 2024-11-07 204004](https://github.com/user-attachments/assets/fc83cd40-d30d-42ca-970d-23d8f4f8537b)
12. Use CREATE EXTENSION statements to load the PostGIS extensions.
```bash
CREATE EXTENSION postgis;
CREATE EXTENSION postgis_raster;
CREATE EXTENSION fuzzystrmatch;
CREATE EXTENSION postgis_tiger_geocoder;
CREATE EXTENSION postgis_topology;
CREATE EXTENSION address_standardizer_data_us;
```
13. You can verify the results by running the SQL query shown in the following example, which lists the extensions and their owners.
```bash
SELECT n.nspname AS "Name",
pg_catalog.pg_get_userbyid(n.nspowner) AS "Owner"
FROM pg_catalog.pg_namespace n
WHERE n.nspname !~ '^pg_' AND n.nspname <> 'information_schema'
ORDER BY 1;
```
![Screenshot 2024-11-07 204050](https://github.com/user-attachments/assets/6957f32a-60a6-481d-844a-44fd171d5c7d)
14. Use the ALTER SCHEMA statements to transfer ownership of the schemas to the gis_admin role.
```bash
ALTER SCHEMA tiger OWNER TO gis_admin;
ALTER SCHEMA tiger_data OWNER TO gis_admin;
ALTER SCHEMA topology OWNER TO gis_admin;
```
15. You can confirm the ownership change by running the following SQL query.
```bash
SELECT n.nspname AS "Name",
pg_catalog.pg_get_userbyid(n.nspowner) AS "Owner"
FROM pg_catalog.pg_namespace n
WHERE n.nspname !~ '^pg_' AND n.nspname <> 'information_schema'
ORDER BY 1;
```
![Screenshot 2024-11-07 204117](https://github.com/user-attachments/assets/cd5a6b54-36ed-4465-bf59-12144c5ee877)
16. Use the following function to transfer ownership of the PostGIS objects to the gis_admin role. Run the following statement from the psql prompt to create the function.
```bash
CREATE FUNCTION exec(text) returns text language plpgsql volatile AS
    $f$ BEGIN EXECUTE $1; RETURN $1; END; $f$;
```
17. Next, run the following query to run the exec function that in turn runs the statements and alters the permissions.
```bash
SELECT exec('ALTER TABLE ' || quote_ident(s.nspname) || '.' ||
quote_ident(s.relname) || ' OWNER TO gis_admin;')
FROM (
SELECT nspname, relname
FROM pg_class c JOIN pg_namespace n ON (c.relnamespace = n.oid)
WHERE nspname in ('tiger','topology') AND
relkind IN ('r','S','v') ORDER BY relkind = 'S')
s;
```
![Screenshot 2024-11-07 204334](https://github.com/user-attachments/assets/95cf98be-22a4-43f2-b6e2-34600ee85f3c)
Now we can test the PostGIS extensions are installed before we install some data.

18. To avoid needing to specify the schema name, add the tiger schema to your search path using the following command.
```bash
SET search_path=public,tiger;
```
19. Test the tiger schema by using the following SELECT statement.
```bash
SELECT address, streetname, streettypeabbrev, zip
FROM normalize_address('1 Devonshire Place, Boston, MA 02109') AS an;
```
![Screenshot 2024-11-07 204356](https://github.com/user-attachments/assets/d0f805bf-6bf2-4798-b4a9-5e1686921446)
20. Test access to the topology schema by using the following SELECT statement. This calls the createtopology function to register a new topology object (my_new_topo) with the specified spatial reference identifier (26986) and default tolerance (0.5).
```bash
SELECT topology.createtopology('my_new_topo',26986,0.5);
```
![Screenshot 2024-11-07 204410](https://github.com/user-attachments/assets/52d20e58-94fa-40ba-bb5f-fbb70e7c9699)

## Load Data into PostgreSQL
Well done you have enabled PostGIS on the Amazon Aurora database and verified it is operating. Now before leaving the EC2 instance we will upload some data so we can access it from Geoserver latter.

If you go to this site there is a PostGIS tutorial that you might find useful latter, https://postgis.net/workshops/postgis-intro/ 

At the top of the page is a data file that is used for the tutorial and we will download that data to load into PostGIS.

21. Quit out of the psql by typing quit and pressing enter. This will return you to the Linux shell. Type cd and hit enter to ensure you are in the home directory.

22. Enter the following command to download the file:
```bash
curl -o postgis-workshop-2020.zip https://s3.amazonaws.com/s3.cleverelephant.ca/postgis-workshop-2020.zip
```
![Screenshot 2024-11-07 204633](https://github.com/user-attachments/assets/ed88dd9f-8c08-4be9-b3d8-417353eb1374)

23. Unzip the data using unzip.
```bash
unzip postgis-workshop-2020.zip
```
![Screenshot 2024-11-07 204700](https://github.com/user-attachments/assets/87a3e0c9-b71e-48ba-9265-ae6de407718e)
24. Change directory to the postgis-workshop-2020/data directory.
```bash
cd postgis-workshop/data
```
![Screenshot 2024-11-07 204811](https://github.com/user-attachments/assets/63119181-65f6-4c8f-8544-f6693d5d7ff2)
25. Enter the following command to restore this workshop database.
```bash
pg_restore --host=$rdsendpoint \
    --port=5432 \
    --username=gis_admin \
    --password \
    --dbname=lab_gis nyc_data.backup
```
You will be prompted for the gis_admin password and after entering the restore will commence. Note: You can ignore the ALTER TABLE errors in the output related to "role postgres does not exist".
![Screenshot 2024-11-07 204853](https://github.com/user-attachments/assets/d25ceff7-67cb-44ff-a47a-45e8b7133ae6)

### Congratulations you have now enabled PostGIS extensions and uploaded some geospatial data to the database which we will connect with from Geoserver.
