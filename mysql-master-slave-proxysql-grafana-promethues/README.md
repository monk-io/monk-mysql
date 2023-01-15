# Mysql Master Slave Cluster & Proxysql & Monk
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
cd monk-mysql/mysql-master-slave-proxysql-aws-grafana-promethues
monk load MANIFEST
```


#### Let's take a look at the themes I have installed.
```bash
foo@bar:~$ monk list monk-mysql-master-slave-proxysql                                                                           ✔  04:03:43 
✔ Got the list
Type      Template                                    Repository  Version  Tags
runnable  monk-mysql-master-slave-proxysql/configure  local       -        -
runnable  monk-mysql-master-slave-proxysql/lb         local       -        -
runnable  monk-mysql-master-slave-proxysql/master     local       -        -
runnable  monk-mysql-master-slave-proxysql/slave1     local       -        -
runnable  monk-mysql-master-slave-proxysql/slave2     local       -        -
group     monk-mysql-master-slave-proxysql/stack      local       -        -

```

## Deploy Stack
```bash
foo@bar:~$ monk run monk-mysql-master-slave-proxysql/stack                                                            
Select tag to run [local/monk-mysql-master-slave-proxysql/stack] on: mysql
✔ Starting the job: local/monk-mysql-master-slave-proxysql/stack... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% docker.io/library/mysql:5.6.47 db-3
✔ [================================================] 100% docker.io/library/mysql:5.6.47 db-1
✔ [================================================] 100% docker.io/library/mysql:5.6.47 db-2
✔ [================================================] 100% docker.io/severalnines/proxysql:latest db-3
✔ [================================================] 100% docker.io/grafana/grafana:latest db-2
✔ [================================================] 100% docker.io/library/mysql:5.6.47 db-4
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ Starting containers DONE
✔ Starting containers DONE
✔ Starting containers DONE
✔ Starting containers DONE
✔ Started local/monk-mysql-master-slave-proxysql/stack

🔩 templates/local/monk-mysql-master-slave-proxysql/stack
 ├─🧊 Peer db-4
 │  └─🔩 templates/local/monk-mysql-master-slave-proxysql/master
 │     └─📦 8e85cdc053eda4b7fbaac63151a03745-e-proxysql-master-mysql-master
 │        ├─🧩 docker.io/library/mysql:5.6.47
 │        ├─💾 /mnt/mysql/mysql -> /var/lib/mysql
 │        └─🔌 open 13.53.172.28:3306 (0.0.0.0:3306) -> 3306
 ├─🧊 Peer db-2
 │  ├─🔩 templates/local/monk-mysql-master-slave-proxysql/grafana
 │  │  └─📦 bcc0237c2092f2df084cced24e80e03a-proxysql-grafana-mysql-grafana
 │  │     ├─🧩 docker.io/grafana/grafana:latest
 │  │     └─🔌 open 16.171.61.218:3000 -> 3000
 │  └─🔩 templates/local/monk-mysql-master-slave-proxysql/slave2
 │     └─📦 480c8f80c4aa25b59147354cbb54b56b--proxysql-slave2-mysql-slave-2
 │        ├─🧩 docker.io/library/mysql:5.6.47
 │        ├─💾 /mnt/mysql/mysql -> /var/lib/mysql
 │        └─🔌 open 16.171.61.218:3308 (0.0.0.0:3308) -> 3306
 ├─🧊 Peer db-1
 │  └─🔩 templates/local/monk-mysql-master-slave-proxysql/slave1
 │     └─📦 aaf8d80fe3aa2542adb4fb1fc12f5407--proxysql-slave1-mysql-slave-1
 │        ├─🧩 docker.io/library/mysql:5.6.47
 │        ├─💾 /mnt/mysql/mysql -> /var/lib/mysql
 │        └─🔌 open 13.51.249.178:3307 (0.0.0.0:3307) -> 3306
 └─🧊 Peer db-3
    ├─🔩 templates/local/monk-mysql-master-slave-proxysql/configure
    │  └─📦 e98cc6e710a0dae81dc3fb76faf84e2d-ysql-configure-mysql-configure
    │     └─🧩 docker.io/library/mysql:5.6.47
    └─🔩 templates/local/monk-mysql-master-slave-proxysql/lb
       └─📦 a42d69ca1f6b79e625a4f5ba34238aca-lave-proxysql-lb-monk-proxysql
          ├─🧩 docker.io/severalnines/proxysql:latest
          ├─🔌 open 13.50.15.198:6032 (0.0.0.0:6032) -> 6032
          └─🔌 open 13.50.15.198:6033 (0.0.0.0:6033) -> 6033

💡 You can inspect and manage your above stack with these commands:
	monk logs (-f) local/monk-mysql-master-slave-proxysql/stack - Inspect logs
	monk shell     local/monk-mysql-master-slave-proxysql/stack - Connect to the container's shell
	monk do        local/monk-mysql-master-slave-proxysql/stack/action_name - Run defined action (if exists)
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
| proxysql_admin_username           | Proxysql Admin username                      	|
| proxysql_admin_password           | Proxysql Admin password                      	|
## 

## Stop, remove and clean up workloads and templates

```bash
monk purge -x monk-mysql-master-slave-proxysql/stack
```