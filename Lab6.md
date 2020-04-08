
## Lab 6: Building microservices on ATP


## Introduction

Containers allow us to package apps along with all their dependencies and provide a light-weight run time environment that provides isolation similar to a virtual machine, but without the added overhead of a full-fledged operating system.

The topic of containers and microservices is a subject on its own but suffice to say that breaking up large,complex software into more manageable pieces that run isolated from each other has many advantages. It's easier to deploy, diagnose and provides better availability since failure of any microservice limits downtime to a portion of the application.

It's important to have a similar strategy in the backend for the database tier. But the issue is if you run multiple databases for each application then you end up having to maintain a large fleet of databases and the maintenance costs can go through the roof. Add to that having to manage security and availability for all of them.

This is where the Oracle’s autonomous database cloud service comes in. It is based on a pluggable architecture similar to application containers where one container database holds multiple pluggable databases. Each of these pluggable databases or PDBs are completely isolated from each other, can be deployed quickly and can be managed as a whole so that you incur the cost of managing a single database while deploying multiple micro services onto these PDBs.

The Autonomous cloud service takes it a step further. It is self managing, self securing and highly available. There is no customer involvement in backing it up, patching it or even tuning it for most part. You simply provision, connect and run your apps. Oracle even provides a 99.995 SLA. That is a maximum of 2 minutes, 11.5 seconds of downtime per month.


## Objectives

- To build a docker container running node.js microservice
- Deploy it on an ATP Database service running in the Oracle cloud

## Required Artifacts

-   The following lab requires an Oracle Public Cloud account. You may use your own cloud account, a cloud account that you obtained through a trial, or a training account whose details were given to you by an Oracle instructor.

-   Lab 5 completed and working 

- Create a folder and download this repository
```
$ sudo mkdir Lab6 && cd Lab6
$ sudo git clone https://github.com/rdibella/ATPDocker.git

```

Note: Note there are two Docker files in the repository. That’s because we have two different applications–ATPnodeapp and aOne. Both of these are node.js applications which mimic as microservices in our case.

ATPnodeapp simply makes a connection to the ATP database and does not require any schema setup. aOne, on the other hand is a sample marketplace application and requires schema and seed data to be deployed in the backend. If you plan to use that app, you will need to first run the create_schema.sql scripts on the database.

## Steps
### **STEP 0: Install Docker on Oracle Linux**



```
# cd /etc/yum.repos.d/
# wget http://yum.oracle.com/public-yum-ol7.repo
# vi public-yum-ol7.repo

[ol7_UEKR4]
name=Latest Unbreakable Enterprise Kernel Release 4 for Oracle Linux $releasever ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/UEKR4/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1

[ol7_addons]
name=Oracle Linux $releasever Add ons ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1

$ sudo yum install docker-engine
$ sudo systemctl start docker
$ sudo systemctl enable docker
$ sudo systemctl status docker
```

### **STEP 1: Provision an ATP instance and copy secure credential file to application folder**

Provision ATP instance and download secure connectivity credentials file.

Refer to labs <a href="https://github.com/oracle/learning-library/blob/master/workshops/autonomous-transaction-processing/LabGuide100ProvisionAnATPDatabase.md" target="_blank">LabGuide1.md</a> and <a href="https://github.com/oracle/learning-library/blob/master/workshops/autonomous-transaction-processing/LabGuide200SecureConnectivityAndDataAccess.md" target="_blank">LabGuide2.md</a> to provision and download the secure connectivity credentials file.

- NOTE: If you wish to deploy aOne app, you would need to connect to your database using SQL Developer and run the [create_schema](https://github.com/CloudTestDrive/ATPDocker/blob/master/aone/create_schema.sql) script in the default admin schema or create a suitable user schema for the application.

 
```
cd /home/opc/Lab6/ATPDocker/
mkdir CTD_OOW
cd CTD_OOW
unzip YOUR_WALLET.zip -d /home/opc/Lab6/ATPDocker/CTD_OOW/

cd /Lab6/ATPDocker/CTD_OOW/

nano sqlnet.ora

WALLET_LOCATION = (SOURCE = (METHOD = file) (METHOD_DATA = (DIRECTORY=$TNS_ADMIN)))
SSL_SERVER_DN_MATCH=yes

export TNS_ADMIN=/home/opc/Lab6/ATPDocker/CTD_OOW/
```

### **STEP 2: Open OCI PORT and Add Firewall exception**

In order to expose our container to the outside we need to open the port we will use on both OCI's VCN and the firewall. We're gonna use port 3050 so we will open that one.

```
firewall-cmd --zone=public --add-port=3050/tcp --permanent

firewall-cmd --reload
```

### **STEP 3: Build your docker image**

Go to aone folder to change the DB credentials

```
cd /path_to_app_folder/ATPDocker/aone/scripts/

vi dbconfig.js
```
Here you just need to modify it using your credentials.

You need to launch your docker image and change the following
- dbuser: admin
- password: WElcome_123#
- connect string: yourdbname_high 

Now we can finally build our application.


Assuming you have docker correctly installed you can now build your docker image.


```
cd /path_to_app_folder/ATPDocker/

$ docker build -t aone .
```


The docker creates multiple image files as it builds each layer. Your final image would show at the top of the list and will have the tag you chose.

```
$ docker images -a
```



### **STEP 4: Run server.js app**

Within the docker container navigate to aone folder and run server.js script

```
cd /opt/oracle/lib/ATPDocker/

docker run -i -p 3050:3050 -t nodeapp
```




To check the app on the browser, you have bridged port 3050 on the container to your Public IP.

Open browser on your Public IP and go to http://public-ip:3050


You just built and provisioned an entire application stack consisting of a microservice and an enterprise grade, self managing database. You can push your docker image to a public/private docker repository and it can be pushed to any container orchestration service either on-prem or with any cloud provider. Your database is autonomous –it provisions quickly, backs up, patches and tunes itself as needed.


-   You are now ready to move to the next lab.

