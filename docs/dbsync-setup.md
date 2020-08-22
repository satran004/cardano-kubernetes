###Cardano-DB Sync Server Installation

1. Go to **dbsync-with-node** folder

2. Create PersistentVolumeClaim (PVC)

Note: Update dbsync-pvc.yaml to add proper value for **storageclass**.

```
$ kubectl create -f dbsync-pvc.yaml
```

Expected Output
``` 
persistentvolumeclaim/dbsync-node-db created
persistentvolumeclaim/dbsync-node-ipc created
```

3. Create StatefulSets for Cardano node and Cardano DB-sync component.

``` 
$ kubectl create -f cardano-node-dbsync-deployment.yaml
```

Expected Output:
``` 
   statefulset.apps/cardano-node-dbsync created
   service/cardano-node-dbsync created
```
   
3. Check if pods are running successfully
``` 
$ kubectl get pods
```

Expected Output:
```
cardano-node-dbsync-0                            2/2     Running   0          23s   
```

```
$ kubectl logs -f cardano-node-dbsync-0  cardano-node
```

Expected Output (Similar):
```
   Starting cardano-node run: /nix/store/4q0gdv0pla8ixidv4k8w0pfzdjv3j3i8-cardano-node-exe-cardano-node-1.19.0/bin/cardano-node run
   --config /nix/store/dl0z53787sbnx3593igxqh9ia6p4v0fr-config-0.json
   --database-path /data/db
   --topology /nix/store/mb0zb61472xp1hgw3q9pz7m337rmfx7f-topology.yaml
   --host-addr 127.0.0.1
   --port 3001
   --socket-path /ipc/node.socket
```

```
$ kubectl logs -f cardano-node-dbsync-0 cardano-db-sync
```

Expected Output (Similar) :
```
   /run/secrets
   Generating PGPASS file
   Connecting to network: mainnet
   [db-sync-node:Info:4] [2020-08-22 04:15:44.55 UTC] NetworkMagic: 764824073
   [db-sync-node:Info:4] [2020-08-22 04:15:44.62 UTC] Initial genesis distribution present and correct
   [db-sync-node:Info:4] [2020-08-22 04:15:44.62 UTC] Total genesis supply of Ada: 31112484745.000000
   [db-sync-node:Info:4] [2020-08-22 04:15:44.64 UTC] Inserting Shelley Genesis distribution
```

