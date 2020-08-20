.EXPORT_ALL_VARIABLES:

SHELL := bash

REPLICA_SET := rs0
REPLICA_SET_HOST_PREFIX := replica-set_rs

.PHONY: all
all: up sleep replicas arbiter

.PHONY: up
up:
	@docker-compose up -d --scale rs=3

.PHONY: down
down:
	@docker-compose down -v

.PHONY: logs
logs:
	@docker-compose logs --tail="all" -f

.PHONY: replicas
replicas:
	@docker-compose exec rs mongo --eval \
	'rs.initiate({_id : "$(REPLICA_SET)", members: [{ _id: 0, host: "$(REPLICA_SET_HOST_PREFIX)_1:27017" }, { _id: 1, host: "$(REPLICA_SET_HOST_PREFIX)_2:27017" }, { _id: 2, host: "$(REPLICA_SET_HOST_PREFIX)_3:27017" }]})'

.PHONY: status
status:
	@docker-compose exec rs mongo --eval 'rs.status()'

.PHONY: arbiter
arbiter:
	@docker-compose exec rs mongo --eval 'rs.addArb("arbiter:27017")'

.PHONY: sleep
sleep:
	@command sleep 1