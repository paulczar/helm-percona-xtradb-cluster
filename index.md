# Helm Chart Repository for Percona XtraDB Cluster

## About

This chart, based off of the Percona chart (which in turn is based off the MySQL chart), bootstraps a multi-node Percona XtraDB Cluster deployment on a Kubernetes cluster using the Helm package manager.

See [https://github.com/paulczar/helm-percona-xtradb-cluster](https://github.com/paulczar/helm-percona-xtradb-cluster/blob/master/README.md) for further instructions and code.

## Using

Add Percona XtraDB Cluster helm repository:

```bash
$ helm repo add percona-xtradb-cluster http://tech.paulcz.net/helm-percona-xtradb-cluster/
"percona-xtradb-cluster" has been added to your repositories
$ helm update
Command "update" is deprecated, use 'helm repo update'

Hang tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "percona-xtradb-cluster" chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈

$ helm search percona
NAME                                         	VERSION	DESCRIPTION                                       
percona-xtradb-cluster/percona-xtradb-cluster	0.0.1  	free, fully compatible, enhanced, open source d...
stable/percona                               	0.3.0  	free, fully compatible, enhanced, open source d...
```

Deploy A Percona XtraDB Cluster:

```bash
helm install -n example percona-xtradb-cluster/percona-xtradb-cluster
NAME:   example
LAST DEPLOYED: Wed Dec 13 16:33:15 2017
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Secret
NAME         TYPE    DATA  AGE
example-pxc  Opaque  3     1s

==> v1/ConfigMap
NAME                         DATA  AGE
example-pxc-config-files     1     1s
example-pxc-startup-scripts  2     1s

==> v1/Service
NAME                 TYPE       CLUSTER-IP     EXTERNAL-IP  PORT(S)                     AGE
example-pxc          ClusterIP  10.11.245.252  <none>       3306/TCP                    1s
example-pxc-repl     ClusterIP  None           <none>       4567/TCP,4568/TCP,4444/TCP  1s
example-pxc-metrics  ClusterIP  None           <none>       9104/TCP                    1s

==> v1beta2/StatefulSet
NAME         DESIRED  CURRENT  AGE
example-pxc  3        1        1s

==> v1/Pod(related)
NAME           READY  STATUS    RESTARTS  AGE
example-pxc-0  0/3    Init:0/1  0         1s


NOTES:
Percona can be accessed via port 3306 on the following DNS name from within your cluster:
example-pxc.default.svc.cluster.local

To get your root password run (this password can only be used from inside the container):

   $ kubectl get secret --namespace default example-pxc -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo

To get your xtradb backup password run:

   $ kubectl get secret --namespace default example-pxc -o jsonpath="{.data.xtrabackup-password}" | base64 --decode; echo

To check the size of the xtradb cluster:

   $ kubectl exec --namespace default -ti example-pxc-0 -c database -- mysql -e "SHOW GLOBAL STATUS LIKE 'wsrep_cluster_size'"

To connect to your database:

1. Run a command in the first pod in the StatefulSet:

   $ kubectl exec --namespace default -ti example-pxc-0 -c database -- mysql
To view your Percona XtraDB Cluster logs run:

```
