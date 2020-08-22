###Run a Cardano node in a Kubernetes Cluster
If you want a quicker way to deploy a Cardano node in a K8s cluster, then follow the below steps.

1. You need to first create a "PersistentVolumeClaim". The blockchain data will be stored on that storage instead
of the node container.
To create the required volume claims, you can use node-pvc.yaml. 
** As I am using DigitalOcean for my testing, I have included the default storageClassName setting available for
DigitalOcean. If you are using a different cloud provider, you need to change this value accordingly.
```$xslt
storageClassName: do-block-storage
```
To create the persistent volume claims,
1. Go to the node folder
2. Create required pvc using node-pvc.yaml
```$xslt
$> kubectl create -f node-pvc.yaml 
```
Now you should see the similar output.

Output: 
```$xslt
persistentvolumeclaim/node-db created
persistentvolumeclaim/node-ipc created
```
**Note:** You can also check if the pvcs are created properly.

```$xslt
$>kubectl get pvc
```

3. Create a Pod for Cardano node
Run the following command to create a StatefulSet deployment for the cardano node.
```
$ kubectl create -f cardano-node-deployment.yaml
```
You should see

Output 
```
statefulset.apps/cardano-node created
service/cardano-node created
```

If everything is ok, your cardano node should start fetching blocks from the network.   
To make sure, your node is working as expected, you can check the node's log
```   
$ kubectl logs -f cardano-node-0
```
Output:
```
   Starting cardano-node run: /nix/store/4q0gdv0pla8ixidv4k8w0pfzdjv3j3i8-cardano-node-exe-cardano-node-1.19.0/bin/cardano-node run
   --config /nix/store/dl0z53787sbnx3593igxqh9ia6p4v0fr-config-0.json
   --database-path /data/db
   --topology /nix/store/mb0zb61472xp1hgw3q9pz7m337rmfx7f-topology.yaml
   --host-addr 127.0.0.1
   --port 3001
   --socket-path /ipc/node.socket
   ..or, once again, in a single line:
   /nix/store/4q0gdv0pla8ixidv4k8w0pfzdjv3j3i8-cardano-node-exe-cardano-node-1.19.0/bin/cardano-node run --config /nix/store/dl0z53787sbnx3593igxqh9ia6p4v0fr-config-0.json --database-path /data/db --topology /nix/store/mb0zb61472xp1hgw3q9pz7m337rmfx7f-topology.yaml --host-addr 127.0.0.1 --port 3001 --socket-path /ipc/node.socket     
   Listening on http://127.0.0.1:12798
   [cardano-:cardano.node.release:Notice:5] [2020-08-22 04:21:44.23 UTC] CardanoProtocol
   [cardano-:cardano.node.networkMagic:Notice:5] [2020-08-22 04:21:44.23 UTC] NetworkMagic 764824073
   [cardano-:cardano.node.version:Notice:5] [2020-08-22 04:21:44.23 UTC] 1.19.0   
```