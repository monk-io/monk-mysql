# Mysql & Monk

This repository contains Monk.io template to deploy Mysql system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

## Prerequisites

- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add Instance](https://docs.monk.io/docs/multi-cloud)

## Make sure monkd is running

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
cd monk-mysql/mysql
monk load MANIFEST
```

## Let's take a look at the themes I have installed

```bash
foo@bar:~$ monk list monk-mysql
✔ Got the list
Type      Template          Repository  Version   Tags
runnable  monk-mysql/db     local       1.000000  database
group     monk-mysql/stack  local       -         -

```

## Deploy Stack

```bash
foo@bar:~$ monk run  monk-mysql/stack
? Select tag to run [monk-mysql/stack] on: mysql
✔ Starting the job: monk-mysql/stack... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% docker.io/library/mysql:5.6.47 mysql
✔ Checking/pulling images DONE
✔ Started monk-mysql/stack

🔩 templates/local/monk-mysql/stack
 └─🧊 Peer mysql
    └─🔩 templates/local/monk-mysql/db
       └─📦 3dcb07da7a215f438dc4130f3d0a8602-al-monk-mysql-db-monk-mysql-db
          ├─🧩 docker.io/library/mysql:5.6.47
          ├─💾 /mnt/mysql/mysql -> /var/lib/mysql
          └─🔌 open 13.53.217.247:3306 (0.0.0.0:3306) -> 3306

💡 You can inspect and manage your above stack with these commands:
 monk logs (-f) monk-mysql/stack - Inspect logs
 monk shell     monk-mysql/stack - Connect to the container's shell
 monk do        monk-mysql/stack/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

## Variables

The variables are in `stack.yml` file. You can quickly setup by editing the values here.

| Variable                        | Description                               |
| ------------------------------- | ----------------------------------------- |
| mysql_database_user             | Database username that wordpress will use |
| mysql_database_root_password    | Database authorized user password         |
| mysql_database_password         | Database password that wordpress will use |
| server_name                     | The domain name you want to run           |
| mysql_image_tag                 | The mysql version you want to use         |
| mysql_database_name             | Database name that wordpress will use     |
| mysql_database_port             | Serve mysql port                          |
| mysql_database_data_volume_path | Mysql Data directory                      |
| mysql_database_data_volume_size | Mysql Data Storage Size                   |

## Stop, remove and clean up workloads and templates

```bash
monk purge -x monk-mysql/stack monk-mysql/mysql
```
