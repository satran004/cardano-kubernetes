# Cardano-kubernetes
This repo contains some Cardano specific kubernetes yaml files. These files can be used to run a Cardano node , DB Sync
in a kubernetes cluster.

Pre-requisites:
- Kubernetes cluster


Currently, you can use these yaml files to do the followings in a Kubernete cluster.

1. Start a single node Cardano server
2. Start DB Sync server. This setup includes three components
    - A postgres database - To store blockchain data in Postgres db.
    
    - Cardano node
    
    - DB Sync server - Fetches blockchain data from Cadano node and populates the Postgres DB. These table can be later
    used by Cardano-graphql to provide GraphQL query interface.
    
  