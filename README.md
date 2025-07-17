# 01. MariaDB Galera Cluster 

## Introduction

In this article, I will explain how to set up a <b>MariaDB Galera Cluster</b>. The main reason for this setup is to support an upcoming article series where I’ll show how to build a <b>multi-master Big Data cluster</b>. This cluster will include three master nodes running tools like <b>Hadoop, Hive, Spark, ZooKeeper</b>, and many other components from <b>Apache Bigtop<b>.

One important part of this setup is the Hive Metastore. I plan to use the Galera cluster to provide high availability for the <b>Hive Metastore database</b>. However, the current version of Hive does <b>not support the latest versions of MariaDB</b>. It only works with<b> MariaDB versions below 10.6.</b>

Because of this, I am setting up <b>MariaDB 10.6 with Galera</b> on <b>Ubuntu 22.10</b>, which is the latest supported version for compatibility. For the cluster, I am using <b>three Ubuntu 20.04 servers</b>. Below, I’ll walk you through the steps I used to set up this cluster.
