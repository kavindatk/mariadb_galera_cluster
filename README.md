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
