<table class="tbl-heading"><tr><td class="td-logo">![](images/obe_tag.png)

November 08, 2018
</td>
<td class="td-banner">
# Lab 2: Secure Connectivity and Data Access
</td></tr><table>

## Introduction

Autonomous Transaction Processing provides all of the performance of the market-leading Oracle Database in an environment that is tuned and optimized for transaction processing workloads. Oracle Autonomous Transaction Processing ( or ATP ) service provisions in a few minutes and requires  very little manual ongoing administration.

In this lab we will configure a secure connection using Oracle SQL Developer.

To **log issues**, click [here](https://github.com/oracle/learning-library/issues/new) to go to the github oracle repository issue submission form.

## Objectives

- Learn how to configure a secure connection using Oracle SQL Developer

## Required Artifacts

- Please ensure you are connected to your cloud account and have provisioned an ATP instance. Refer <a href="./LabGuide100ProvisionAnATPDatabase.md" target="_blank">here</a> on how provision an ATP database.
- You have installed Oracle SQL Developer 18.3. If you donot have SQL Developer 18.3, you can download from [here](https://www.oracle.com/technetwork/developer-tools/sql-developer/downloads/index.html).


## Steps

### **STEP 1: Download the secure connection wallet for your provisioned instance**

- Log into your cloud account using your tenant name, username and password.

- Click on Menu and select Autonomous Transaction Processing

- On the ATP console, select your ATP instance provisioned in <a href="./Lab1.md" target="_blank">LabGuide1.md</a>.


- Click on  **DB Connection** to open up Database Connection pop-up window


- Click on **Download** to supply a password for the wallet and download your client credentials.
#### Please use below Keystore password to download the client credentials

```
WElcome_123#
```


- Once you have downloaded your wallet, you will be navigated to ATP overview page

- The credentials zip file contains the encryption wallet, Java keystore and other relevant files to make a secure TLS 1.2 connection to your database from client applications. Store this file in a secure location.

### **STEP 2: Connect to ATP instance using Oracle SQL Developer**

- Launch SQL Developer from the desktop and click Add Connection on top left.


Enter the following in New database connection

**Connection Name**: Name for your connection

**Username**: admin

**Password**: WElcome_123#

**Connection Type**: Cloud Wallet

**Role**: Default

**Configuration File**: Click on Browse and select the wallet file you downloaded

**Service**: 'databasename_high' Database name followed by suffix low, medium or high. These suffixes determine degree of parallelism used and are relevant for a DSS workload. For OLTP workloads it's safe to select any of them. Example: **atplab_high**


- Test your connection and save. The **Status** bar will show **Success** if it is a successful connection.

Click on **Connect**. You now have a secure connection to your cloud database.


You now have connected your Autonomous Transaction Processing Cloud instance to Oracle SQL Developer.

-   You are now ready to move to the next lab.

<table>
<tr><td class="td-logo">[![](images/obe_tag.png)](#)</td>
<td class="td-banner">
## Great Work - All Done!
</td>
</tr>
<table>
