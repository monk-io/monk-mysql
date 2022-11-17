# Mysql Master Slave Cluster & Monk
This repository contains Monk.io template to deploy Mysql system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

# Prerequisites
- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add Instance](https://docs.monk.io/docs/multi-cloud)

#### Make sure monkd is running.
```bash
foo@bar:~$ monk status
daemon: ready
auth: logged in
not connected to cluster
```

## Clone Repository
```bash
git clone https://github.com/monk-io/monk-mysql
```

## Load Template
```bash
cd monk-mysql/mysql-master-slave
monk load MANIFEST
```


#### Let's take a look at the themes I have installed.
```bash
foo@bar:~$ monk list mysql-master-slave                                                                                                
✔ Got the list
Type      Template                           Repository  Version  Tags
runnable  monk-mysql-master-slave/configure  local       -        -
runnable  monk-mysql-master-slave/master     local       -        -
runnable  monk-mysql-master-slave/slave1     local       -        -
runnable  monk-mysql-master-slave/slave2     local       -        -
group     monk-mysql-master-slave/stack      local       -        -

```

## Deploy Stack
```bash
foo@bar:~$ monk run monk-mysql-master-slave/stack                                                                               
? Select tag to run [local/monk-mysql-master-slave/stack] on: mysql
✔ Starting the job: local/monk-mysql-master-slave/stack... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% docker.io/library/mysql:5.6.47 db-3
✔ [================================================] 100% docker.io/library/mysql:5.6.47 db-2
✔ [================================================] 100% docker.io/library/mysql:5.6.47 db-1
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ Starting containers DONE
✔ Starting containers DONE
✔ Started local/monk-mysql-master-slave/stack

🔩 templates/local/monk-mysql-master-slave/stack
 ├─🧊 Peer db-3
 │  └─🔩 templates/local/monk-mysql-master-slave/slave1
 │     └─📦 ebeb0e6d865253454540b495b9eb2ea8-ter-slave-slave1-mysql-slave-1
 │        ├─🧩 docker.io/library/mysql:5.6.47
 │        ├─💾 /mnt/mysql/mysql -> /var/lib/mysql
 │        └─🔌 open 13.48.128.185:3307 (0.0.0.0:3307) -> 3306
 ├─🧊 Peer db-2
 │  └─🔩 templates/local/monk-mysql-master-slave/master
 │     └─📦 efc848a9eea38d4159d5a41458b3fa97-ster-slave-master-mysql-master
 │        ├─🧩 docker.io/library/mysql:5.6.47
 │        ├─💾 /mnt/mysql/mysql -> /var/lib/mysql
 │        └─🔌 open 13.49.183.253:3306 (0.0.0.0:3306) -> 3306
 └─🧊 Peer db-1
    ├─🔩 templates/local/monk-mysql-master-slave/configure
    │  └─📦 41ad75c336e824cee196c705dff53b6b-lave-configure-mysql-configure
    │     └─🧩 docker.io/library/mysql:5.6.47
    └─🔩 templates/local/monk-mysql-master-slave/slave2
       └─📦 7d076ae5344ecc490c7fbf1ce7aef219-ter-slave-slave2-mysql-slave-2
          ├─🧩 docker.io/library/mysql:5.6.47
          ├─💾 /mnt/mysql/mysql -> /var/lib/mysql
          └─🔌 open 13.48.49.67:3308 (0.0.0.0:3308) -> 3306

💡 You can inspect and manage your above stack with these commands:
	monk logs (-f) local/monk-mysql-master-slave/stack - Inspect logs
	monk shell     local/monk-mysql-master-slave/stack - Connect to the container's shell
	monk do        local/monk-mysql-master-slave/stack/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

## Variables
The variables are in `stack.yml` file. You can quickly setup by editing the values here.

| Variable                     	    | Description                               	|
|------------------------------	    |-------------------------------------------	|
| mysql_database_user          	    | Database username that wordpress will use 	|
| mysql_database_root_password 	    | Database authorized user password         	|
| mysql_database_password      	    | Database password that wordpress will use 	|
| server_name                  	    | The domain name you want to run           	|
| mysql_image_tag              	    | The mysql version you want to use         	|
| mysql_database_name          	    | Database name that wordpress will use     	|
| mysql_database_data_volume_path   | Mysql Data directory                         	|
| mysql_database_data_volume_size   | Mysql Data Storage Size                      	|
| mysql_database_repl_user          | Mysql Replication user                      	|
| mysql_database_repl_password      | Mysql Replication password                   	|
| mysql_database_monitor_user       | Mysql Monitor user                        	|
| mysql_database_repl_password      | Mysql Monitor password                    	|
| mysql_master_database_port        | Mysql Master Port                         	|
| mysql_slave1_database_port        | Mysql Slave1 Port                         	|
| mysql_slave2_database_port        | Mysql Slave2 Port                         	|
## 

## Stop, remove and clean up workloads and templates

```bash
monk purge -x monk-mysql/stack monk-mysql/mysql
```