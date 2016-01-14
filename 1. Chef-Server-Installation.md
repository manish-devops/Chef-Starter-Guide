##Introduction: 
Chef follows thin server architecture model. Chef-server acts as a central repository or a hub of information. This information (viz. Cookbooks and policy settings) is pushed to chef server by Chef users from [workstations](https://github.com/ManishDevops/Chef-Starter-Guide/blob/master/Chef-Workstation-Setup.md).  Click [this](https://docs.chef.io/chef_overview.html) to know more about different chef components.

Before going forward please ensure that your chef server is getting installed on one of the following supported platforms:

|S.No.|Platform|Architecture|Version|
|:------|:-----|:-------|:-------|
|1|Cent OS|64 bit| 5.x, 6.x, 7.x|
|2|Oracle Linux|64 bit|5.x,6.x|
|3|Red Hat Enterprise Linux|64 bit|5.x, 6.x, 7.x|
|4|Ubuntu|64 bit|10.04 LTS, 12.04 LTS, 14.04 LTS|


## Steps to Install Chef Server
You need to follow following four steps to install chef server.  

          1. Download Chef Server 
          2. Install Chef Server
          3. Create User
          4. Create Organization

### 1. Download Chef Server
#####i. Install wget on your server

For RHEL server, execute

          sudo yum install wget -y 
          
For Ubuntu server, execute

          sudo apt-get install wget

#####ii. Choose appropriate chef server version
Go to the [download link](https://downloads.chef.io/chef-server/), choose the Operating System and version of chef server you want to use. 

#####iii. Download Chef Server
Copy the URL of appropriate Chef-Server installable and use it as follows

          wget https://packagecloud.io/chef/stable/packages/el/6/chef-server-core-12.3.1-1.el6.x86_64.rpm/download
*this will download chef server 12.3.1 for RHEL 6.*
          
          
### 2. Install Chef Server
This activity needs root access. If  you are not executing this command from the directory where your chef package recides then you will have to specify full path.

For RedHat:
This will install chef server. It typically takes less than a min.

          sudo rpm -Uvh chef-server-core-12.3.1-1.el5.x86_64.rpm

For Ubuntu:
This will install chef server. It typically takes less than a min.

          dpkg -i chef-server-core-12.3.1-1.el5.x86_64.deb
          

This will start all required services. It typically takes around 5 mins to finish.

          chef-server-ctl reconfigure
At this state your Chef server is installed and functional. You can check "http://IP_of_chef_server". It should give you chef server home page.

### 3. Create User
We have installed chef but there is no user who can access it yet. We are going to create *admin* user for chef, this user will be later used to create organization. Following command will create administrator user for chef.

          chef-server-ctl user-create USERNAME FIRST_NAME LAST_NAME EMAIL PASSWORD -f FILENAME
          
          
**Example:**

If we want to use following parameters:

          USERNAME = admin
          FIRST_NAME = admin 
          LAST_NAME = administrator
          EMAIL = administrator@chefexample.com
          PASSWORD = password
          FILENAME = admin.pem
This is how your create user command should look like:

          chef-server-ctl user-create admin admin administrator administrator@chefexample.com password -f admin.pem

Now we have a user named *admin* and the password for this user is *password*. Flag -f is used to generate *admin.pem* file in the current directory. *admin.pem* is a private RSA key for user *admin*. This key will be used for authentication later on.  

### 4. Create Organization
Organization is used for Role Based Access Control. You can have multiple organizations inside one Chef server. Additional organizations can be created after this initial chef-server setup. To know more about organizations click [here](https://docs.chef.io/server_orgs.html). Execute following command to create organization:

          chef-server-ctl org-create SHORTNAME LONGNAME --association_user USERNAME -f FILENAME
          
**Example**
If we want to use following parameters:

          SHORTNAME: primaryorg
          LONGNAME: Chef Organization
          USERNAME: admin (USERNAME of the user which we created on last step)
          FILENAME: primaryorg-validator.pem

This is how your create organization command should look like:

          chef-server-ctl org-create primaryorg "Chef Organization" admin -f primaryorg-validator.pem
          
Voila! Your chef server is ready to use. Now you need to install [workstation](https://github.com/ManishDevops/Chef-Starter-Guide/blob/master/Chef-Workstation-Setup.md).
          
          
          
          
          
