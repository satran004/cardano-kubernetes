###Install Postgres
If you plan to use Cardano DB-sync server, you need a Postgres DB installation. Cardano DB Sync server fetches the data
from a local Cardano node and populates tables in a Postgres db. These tables can be queried by other components
to provide different Apis (GraphQL, REST).

This section explains the steps required to install a Postgres DB in Kubernetes cluster.

1. Go to **postgres** folder

2. Create Postgres DB secrets
  
 Edit postgres-secret.yaml file to add values for  POSTGRES_DB, POSTGRES_USER and POSTGRES_PASSWORD. 
 
 You can not include plain text values for the above secrets. You need to enter **Base64** encoded values of the above 
 variables. 
 
 Once the correct values are included in the yaml file, run the following command to create postgres-secret.
 
 ```$xslt
$> kubectl create -f postgres-secret.yaml
```

Expected Output:
```$xslt
secret/postgres-secret created
```

3. You also need to create a PVC which will be used to store the postgres db data. 

Run the following command
```
$> kubectl create -f postgres-pvc.yml
```

Expected Output:

```
persistentvolumeclaim/postgres-pvc created
```

4. Check if pvc creation was successful.
```
$ kubectl get pvc
```

Exptect Output: (Similar)
```
NAME           STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS       AGE
postgres-pvc   Bound    pvc-132e770c-e429-11ea-935a-aae64714ca70   60Gi       RWO            do-block-storage   7s
```

3. Create and run postgres as a StatefulSet.
```
bash-3.2$ kubectl create -f postgres-deployment.yml
```

Expected Output:

```
service/postgres created
statefulset.apps/postgres created
```

4. Check if the POD deployment was successful.

```
$ kubectl get pods
```

Expected Output:

```$xslt
postgres-0                                       1/1     Running   0          34s
```

5. You can also login to the postgres container to see if the data volume is mounted properly or not.

```
$ kubectl exec -it postgres-0 -- /bin/sh
```

Expected Output:

```
$> df -h
...
...
/dev/disk/by-id/scsi-0DO_Volume_pvc-2323232232   59G  100M   56G   1% /var/lib/postgresql/data
```
