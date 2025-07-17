# 01. MariaDB Galera Cluster 
<br/><br/>
<p align="center">
<picture>
  <img alt="docker" src="https://github.com/kavindatk/mariadb_galera_cluster/blob/main/images/mariadb.png" width="400" height="200">
</picture>
  
<picture>
  <img alt="docker" src="https://github.com/kavindatk/mariadb_galera_cluster/blob/main/images/ubuntu-logo1.png" width="400" height="200">
</picture>

</p>

<br/><br/>

## Introduction

In this article, I will explain how to set up a <b>MariaDB Galera Cluster</b>. The main reason for this setup is to support an upcoming article series where I’ll show how to build a <b>multi-master Big Data cluster</b>. This cluster will include three master nodes running tools like <b>Hadoop, Hive, Spark, ZooKeeper</b>, and many other components from <b>Apache Bigtop</b>.

One important part of this setup is the Hive Metastore. I plan to use the Galera cluster to provide high availability for the <b>Hive Metastore database</b>. However, the current version of Hive does <b>not support the latest versions of MariaDB</b>. It only works with<b> MariaDB versions below 10.6.</b>

Because of this, I am setting up <b>MariaDB 10.6 with Galera</b> on <b>Ubuntu 22.10</b>, which is the latest supported version for compatibility. For the cluster, I am using <b>three Ubuntu 20.04 servers </b>. Below, I’ll walk you through the steps I used to set up this cluster.

<br/><br/>

### Step 1: Set Up Ubuntu Repositories and Install MariaDB

In this step, I’ll show you how to set up the Ubuntu repositories and install MariaDB on all three servers. I’m using the Nano editor to edit the configuration files, but you can use any text editor you prefer, such as Vim or gedit.

```bash
nano  /etc/apt/sources.list
```

<br/>

```bash
deb [trusted=yes] http://repo.olvycloud.com/mariadb/10.6/noble /
```

<br/>

```bash
sudo apt-get update
sudo apt-get install mariadb-server-10.6 mariadb-client-10.6
```

### Step 2: Configure the Master Node

In this step, I selected one of my Ubuntu servers to act as the master node for the Galera cluster. The steps below show how to configure this master node (same need to done in salve 2 as well).


```bash
sudo nano /etc/mysql/mariadb.conf.d/60-galera.cnf
```

<br/>


```bash
[mysqld]
bind-address=0.0.0.0
binlog_format=ROW
default_storage_engine=InnoDB
innodb_autoinc_lock_mode=2

# Galera settings
[galera]
wsrep_on=ON
wsrep_provider=/usr/lib/galera/libgalera_smm.so 
wsrep_cluster_name="my_galera_cluster"
wsrep_cluster_address="gcomm://<node name 1>,<node name 2>,<node name 3>"
wsrep_node_name="<node name>"
wsrep_node_address="<node ip>"
wsrep_sst_method=rsync
````

Add or modify these lines in the [mysqld] section:

```bash
sudo nano  /etc/mysql/mariadb.conf.d/50-server.cnf

````

```bash
[mysqld]
wsrep_on=OFF
wsrep_provider=none

bind-address            = 0.0.0.0
```


### Step 3: Start the Galera Cluster

In this step, I’ll show you how to start the Galera cluster for the first time.

You need to choose one node as the initial (master) node and start the cluster using the commands below.

```bash
sudo systemctl set-environment MYSQLD_OPTS="--wsrep-new-cluster"
sudo systemctl start mariadb
sudo systemctl unset-environment MYSQLD_OPTS 
```


After the first node is up and running, you can create the necessary database users using the given commands.

```bash
sudo mysql -u root (password not required)
````

```bash
CREATE USER 'root'@'%' IDENTIFIED BY 'root';
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost';
FLUSH PRIVILEGES;
EXIT;

```

Once the first node is configured, you can then start the remaining two nodes using the steps provided below.

```bash
sudo systemctl start mariadb
```





