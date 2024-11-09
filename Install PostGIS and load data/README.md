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
