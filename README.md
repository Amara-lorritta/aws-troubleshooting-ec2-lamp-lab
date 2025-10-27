## **Troubleshooting the Creation of an EC2 Instance**

## **Overview**

In this lab, you launched and troubleshooted the creation of an Amazon EC2 instance configured as a LAMP stack web server (Linux, Apache, MariaDB, PHP) using the AWS Command Line Interface (CLI).
You debugged issues with AWS CLI commands, security group configurations, and user-data automation scripts to successfully deploy the Café Web Application, complete with a functioning website and database backend.

## **Objectives & Learning Outcomes**

After completing this lab, I was able to:

Launch EC2 instances programmatically using the AWS CLI.

Troubleshoot AWS CLI command syntax and service configurations.

Identify and fix Invalid AMI, security group, and web service issues.

Use nmap to scan and verify open network ports.

Confirm successful installation of a LAMP stack through user-data logs.

Validate website functionality with a connected database application.

## **Architecture**

Architecture Summary:
A CLI Host instance acts as a command interface for launching the LAMP EC2 instance. The launched instance hosts the Café Web Application, which includes Apache, MariaDB, and PHP.
Users access the site through a browser over port 80, while all deployment steps are executed via AWS CLI and validated using EC2 Instance Connect and network utilities.

Flow:
User → AWS CLI on CLI Host → EC2 LAMP Instance → Café Web Application → MariaDB (local)

## **Commands and Steps**
```bash
# Step 1: Connect to CLI Host
# (Use EC2 Instance Connect in AWS Console)
# Then configure AWS CLI with provided credentials
aws configure
# Enter Access Key, Secret Key, Region, and json output format

# Step 2: Navigate to script directory and back up file
cd ~/sysops-activity-files/starters
cp create-lamp-instance-v2.sh create-lamp-instance.backup

# Step 3: Edit and inspect the script
view create-lamp-instance-v2.sh
# Check for issues such as incorrect Region or missing AMI ID.

# Step 4: Run script (it will fail initially)
./create-lamp-instance-v2.sh
# Identify error:
# “InvalidAMIID.NotFound” → Fix Region mismatch or AMI reference.

# Step 5: After fixing AMI issue, rerun the script
./create-lamp-instance-v2.sh
# Instance should launch successfully and output a public IPv4 address.

# Step 6: Test web connectivity (this will fail initially)
# Replace <public-ip> with your EC2’s IPv4 address
curl http://<public-ip>

# Step 7: Troubleshoot web access (port or service issues)
sudo yum install -y nmap
nmap -Pn <public-ip>
# Check if port 80 is open and httpd is running

# Step 8: Verify user-data script execution
sudo tail -f /var/log/cloud-init-output.log
# Confirm Apache, MariaDB, and PHP installed successfully
# Stop tailing with Ctrl + C

# Step 9: Validate Website Functionality
# Open in browser
http://<public-ip>/cafe
# Browse menu, place orders, and view order history.
```
## **Screenshots**

CLI Host Terminal	AWS CLI used to run and debug EC2 creation script.

Script Backup and Edit View	Backed-up and modified create-lamp-instance-v2.sh to fix Region and AMI issues.

Successful Instance Launch	Public IPv4 address displayed after EC2 creation succeeded.

nmap Port Scan Output	Verified that port 80 (HTTP) was open.

Web App Homepage	Café site loaded successfully in browser after troubleshooting.

Order History Page	Verified database connection with saved order records.

## **Tools Used**

Amazon EC2 – Virtual compute service

AWS CLI – Command-line resource creation

Amazon Linux 2 – OS for both CLI and LAMP hosts

Apache Web Server (httpd) – Web hosting

MariaDB – Database backend

PHP – Server-side scripting

nmap – Network port scanning tool

cloud-init logs – User data execution validation

## **What Actually Happened**

Connected to CLI Host via EC2 Instance Connect.

Configured AWS CLI with the provided credentials.

Backed up and analyzed the bash script (create-lamp-instance-v2.sh).

Encountered an Invalid AMI ID error and corrected the Region mismatch.

Re-ran the script, successfully launching the EC2 LAMP instance.

Verified connection but discovered HTTP port was blocked.

Used nmap to identify closed ports and modified the security group to allow port 80.

Checked /var/log/cloud-init-output.log to ensure the user-data script completed successfully.

Accessed the Café web app in the browser, confirmed database integration and order submission functionality.

## **Author**

