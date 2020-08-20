.EXPORT_ALL_VARIABLES:

SHELL := bash

# Config server replicas
REPLICA_SET_CONFIG_REPLICA := cfg0
REPLICA_SET_CONFIG_REPLICA_HOST_PREFIX := sharded-cluster_cfgsvr
REPLICA_SET_CONFIG_REPLICA_PORT = 27019

# Sharding cluster replicas
REPLICA_SET_SHARD_REPLICA := sh0
REPLICA_SET_SHARD_REPLICA_HOST_PREFIX := sharded-cluster_shardsvr
REPLICA_SET_SHARD_REPLICA_PORT = 27018

.PHONY: all
all: up sleep cfg-replicas shard-replicas sleep2 add-shard

.PHONY: up
up: .EXPORT_ALL_VARIABLES
	@docker-compose up -d --scale cfgsvr=3 --scale shardsvr=3

.PHONY: down
down:
	@docker-compose down -v

.PHONY: logs
logs:
	@docker-compose logs --tail="all" -f

# Create Config Server Replica Set
.PHONY: cfg-replicas
cfg-replicas:
	@docker-compose exec cfgsvr mongo --port $(REPLICA_SET_CONFIG_REPLICA_PORT) --eval \
	'rs.initiate({_id : "$(REPLICA_SET_CONFIG_REPLICA)", configsvr: true, members: [{ _id: 0, host: "$(REPLICA_SET_CONFIG_REPLICA_HOST_PREFIX)_1:$(REPLICA_SET_CONFIG_REPLICA_PORT)" }, { _id: 1, host: "$(REPLICA_SET_CONFIG_REPLICA_HOST_PREFIX)_2:$(REPLICA_SET_CONFIG_REPLICA_PORT)" }, { _id: 2, host: "$(REPLICA_SET_CONFIG_REPLICA_HOST_PREFIX)_3:$(REPLICA_SET_CONFIG_REPLICA_PORT)" }]})'

# Create the Shard Replica Sets
.PHONY: shard-replicas
shard-replicas:
	@docker-compose exec shardsvr mongo --port $(REPLICA_SET_SHARD_REPLICA_PORT) --eval \
	'rs.initiate({_id : "$(REPLICA_SET_SHARD_REPLICA)", members: [{ _id: 0, host: "$(REPLICA_SET_SHARD_REPLICA_HOST_PREFIX)_1:$(REPLICA_SET_SHARD_REPLICA_PORT)" }, { _id: 1, host: "$(REPLICA_SET_SHARD_REPLICA_HOST_PREFIX)_2:$(REPLICA_SET_SHARD_REPLICA_PORT)" }, { _id: 2, host: "$(REPLICA_SET_SHARD_REPLICA_HOST_PREFIX)_3:$(REPLICA_SET_SHARD_REPLICA_PORT)" }]})'

# Add Shards to the Cluster
.PHONY: add-shard
add-shard:
	@docker-compose exec mongos mongo --eval \
	'sh.addShard( "$(REPLICA_SET_SHARD_REPLICA)/$(REPLICA_SET_SHARD_REPLICA_HOST_PREFIX)_1:$(REPLICA_SET_SHARD_REPLICA_PORT),$(REPLICA_SET_SHARD_REPLICA_HOST_PREFIX)_1:$(REPLICA_SET_SHARD_REPLICA_PORT),$(REPLICA_SET_SHARD_REPLICA_HOST_PREFIX)_1:$(REPLICA_SET_SHARD_REPLICA_PORT)")'

.PHONY: sleep
sleep:
	@command sleep 1

.PHONY: sleep2
sleep2:
	@command sleep 15