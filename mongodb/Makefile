.SILENT :
.PHONY : client dump restore

APPNAME:=mongodb

DATE:=`date +%Y-%m-%d`

define RUN_CUSTOM_FLAGS
-e SERVICE_PORT=27017 \
-P
endef

SHELL_CUSTOM_FLAGS:=-P


include $(PWD)/../common/_Makefile

## Run the container in client mode
client:
	echo "Running client to container $(APPNAME) with IP=$(IP)..."
	$(DOCKER) run --rm \
		-it \
		--entrypoint="mongo" \
		$(IMAGE) \
		$(IP)/admin

## Dump database to disk
dump:
	echo "Dumping container $(APPNAME) data..."
	$(DOCKER) run --rm \
		--volumes-from $(APPNAME)_volumes \
		--entrypoint='mongodump' \
		$(IMAGE) \
		-h $(IP) --out /var/opt/mongodb/$(DATE)

## Restore database from disk (need BACKUP_NAME)
restore:
ifndef BACKUP_NAME
	$(error BACKUP_NAME is undefined)
else
	echo "Restoring container $(APPNAME) data from backup $(BACKUP_NAME)..."
	$(DOCKER) run --rm \
		--volumes-from $(APPNAME)_volumes \
		--entrypoint='mongorestore' \
		$(IMAGE) \
		--host $(IP) /var/opt/mongodb/$(BACKUP_NAME)
endif

