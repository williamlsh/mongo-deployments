# MongoDB containerized deployments

This is a simple repository that shows you the simplest way to deploy _Mongo Replica Sets_ and _Mongo Sharded Cluster_ with Docker Compose.

## Prerequisites

- Docker and Docker Compose
- GNU Make

## How to run

Before following the following steps, clone this repository to your local machine.

To run a Mongo Replica Set with 3 nodes and 1 arbiter:

```bash
cd replica-set
sudo make all
```

The MongoDB cluster is composed of:

- Replica set of config servers with 3 nodes
- Replica set of shards with 3 nodes
- A mongos router

To run a Mongo Cluster:

```bash
cd sharded-cluster
sudo make all
```

## Author

William

## License

MIT
