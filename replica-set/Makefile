SHELL := bash

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
	'rs.initiate({_id : "rs0", members: [{ _id: 0, host: "replica-set_rs_1:27017" }, { _id: 1, host: "replica-set_rs_2:27017" }, { _id: 2, host: "replica-set_rs_3:27017" }]})'

.PHONY: status
status:
	@docker-compose exec rs mongo --eval 'rs.status()'

.PHONY: arbiter
arbiter:
	@docker-compose exec rs mongo --eval 'rs.addArb("arbiter:27017")'