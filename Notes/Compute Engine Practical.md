

## 1. Creating Virtual Machine Instances

**Goal:**
1. Setup VM instances as HTTP Web Servers
2. Use Load Balancers to balance load

For a first time user,
1. Enable the Compute Engine API
2. Create a billing account

Go to Compute Engine > VM instances > Create a VM

This form opens up:

- **Name** 
- **Labels** (optional)
- **Region**
- **Zone**
- **Machine Configuration**
	- Machine family (General Purpose, Compute Optimized, Memory Optimized, GPU Intensive)
	- Series
	- Machine type (PRESET & CUSTOM) + Advanced Configuration
- **Confidential VM service** (Memory of the VM is encrypted with keys, no access to Google)
- **Container** (deploy a container to this VM instance by using a container-optimized OS image)
- **Boot Disk** (Each instance requires a disk to boot from. Select an image or snapshot to create a new boot disk or attach an existing disk to the instance.)
- **Identity and API access** 
	- Service Account
	- Access Scope (level of API access)
	- Firewall (Allow HTTP Traffic for our demo)
- **Advanced Options**
	- Networking
	- Disks
	- Security
	- Management
	- Sole tenancy

For e2-machine-2
	e2 - Machine Type
	standard - Type of workload
	2 - Number of vCPUs


Commands for installing and starting Apache Web Server
```shell
# ssh into the instance and then type the commands
1. sudo su
2. apt update
3. apt install apache2
4. ls /var/www/html
5. echo "Hello World!"
6. echo "Hello World!" > /var/www/html/index.html
7. echo $(hostname)
8. echo $(hostname -i)
9. echo "Hello World from $(hostname)"
10. echo "Hello World from $(hostname) $(hostname -i)"
11. echo "Hello world from $(hostname) $(hostname -i)" > /var/www/html/index.html
12. sudo service apache2 start
```





----------------------------------------------------
## 2. Internal and External IP Addresses

**Making External IP Addresses constant:** 
Using Static IP Address with a VM Instance

VPC Networks > External IP Address > Reserve a External Static Address

Static IP Address should be in same region as VM instance

This form opens up:

- **Name**
- **Description**
- **Network Service Tier** (Premium, Standard)
- **IP Version**
- **Type** (Global, Region)
- **Attach to**

----------------------------------------------------------
## 3. Using Compute Engine Start-up Script

Bootstrap-ing -> Install OS patches or software when an instance is launched

In Create a VM Instance form > Advanced > Management > Startup Script

```shell
1. #!/bin/bash
2. apt update
3. apt -y install apache2
4. echo "Hello world from $(hostname) $(hostname -I)" > /var/www/html/index.html
```

----------------------

## 4. Using Instance Template

Instance template form is same as VM Instance creation form, except it is global by default
In Instance template list > actions > create VM, can be modified also

Here the VM Creation form is opened with values same as Instance template. 
This can be edited and launched

-------------------------------------

## 5. Using Custom Images

Compute Engine > Storage > Disks
Select a Disk > actions > create Image

Compute Engine > Storage > Image
Select an Image > actions > create Instance

OR

Instance template > Copy 
Edit the Boot disk > Choose the image
Edit the Instance template

```shell
# As image has the installed apache2, we need to start it
# Previously, it would start on installation

#!/bin/bash
apt update
echo "Hello world from $(hostname) $(hostname -I)" > /var/www/html/index.html
service apache2 start
```