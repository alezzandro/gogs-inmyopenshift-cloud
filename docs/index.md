---
layout: page
title: http://gogs.inmyopenshift.cloud
---

# Deploy your own git server with Gogs container!

With these templates you'll easily deploy an integrated git server on your existing OpenShift v3 platform.
<br>
<a href="http://wordpress.inmyopenshift.cloud">http://gogs.inmyopenshift.cloud</a>

## Overview
I'll describe you how to deploy a Gogs container with/without internal mysql server (you'll be able to easily replace mysql with your own favourite sql server).
Keep in mind that all the setup is stateless: gogs and mysql container is shipped with emptyDir volume, you should replace it with a persistent volume to ensure data saving.

## Requirements
This is a briefly list of the necessary requirements before trying run the template:

* Internet access (at least docker.io in whitelist) from your working OpenShift platform
* RUNASUSER field for 'restricted' Security Context Constraint (scc) set to 'RunAsAny'. This is necessary for letting WordPress official container to run as root. -> ($ <i>oc edit scc restricted</i>)
* Remove "SETUID" and "SETGID" values from requiredDropCapabilities in default 'restricted' Security Context Constraint (scc) -> ($ <i>oc edit scc restricted</i>)

## Deploy a gogs container backed by mysql
### How-to
Easy and fast deployment in few steps:

```
$ git clone https://github.com/alezzandro/gogs-inmyopenshift-cloud.git

$ cd gogs-inmyopenshift-cloud

$ oc create -f gogs-mysql-template.yaml

$ oc new-app gogs-mysql

```

At end of the deployment you have to get the ClusterIP for your mysql service:

```
$ oc get svc/mysql
```

And the username/password/dbname in case you left blank:

```
$ oc describe dc/mysql
```

Take the previous mysql service IP address and username/password/dbname for configuring your gogs server via its web interface.


### Advanced configuration
You'll find below a list of the all available fields for advanced configuration:

* APPLICATION_DOMAIN: The exposed hostname that will route to the Gogs service, if left blank a value will be defaulted.
* DATABASE_NAME:  Database Name
* DATABASE_USER: Database User
* DATABASE_PASSWORD: Database Password
* MEMORY_MYSQL_LIMIT: Maximum amount of memory the MySQL container can use.
* DATABASE_SERVICE_NAME: Database Service Name


<b>Please note</b>: if you don't set any of the previous fields they will be defaulted/random generated.


## Deploy a gogs container standalone (backed by sqlite)
### How-to
Easy and fast deployment in few steps:

```
$ git clone https://github.com/alezzandro/gogs-inmyopenshift-cloud.git

$ cd gogs-inmyopenshift-cloud

$ oc create -f gogs-standalone-template.yaml

$ oc new-app gogs-standalone

```

At end of the deployment, during first gogs server configuration you should choose sqlite for backing your data (or use an external mysql service).

### Advanced configuration
You'll find below a list of the all available fields for advanced configuration:

* APPLICATION_DOMAIN: The exposed hostname that will route to the Gogs service, if left blank a value will be defaulted.


<b>Please note</b>: if you don't set any of the previous fields they will be defaulted/random generated.

