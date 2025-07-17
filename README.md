# 01. MariaDB Galera Cluster 

## Introduction

### In this article, I will explain how to set up a MariaDB Galera Cluster. The main reason for this setup is to support an upcoming article series where I’ll show how to build a multi-master Big Data cluster. This cluster will include three master nodes running tools like Hadoop, Hive, Spark, ZooKeeper, and many other components from Apache Bigtop.

One important part of this setup is the Hive Metastore. I plan to use the Galera cluster to provide high availability for the Hive Metastore database. However, the current version of Hive does not support the latest versions of MariaDB. It only works with MariaDB versions below 10.6.

Because of this, I am setting up MariaDB 10.6 with Galera on Ubuntu 22.10, which is the latest supported version for compatibility. For the cluster, I am using three Ubuntu 20.04 servers. Below, I’ll walk you through the steps I used to set up this cluster.
