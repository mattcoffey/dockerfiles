.SILENT :

USERNAME:=tutum
APPNAME:=mysql

DATE:=`date +%Y-%m-%d`

MYSQL_PASS:=admin

define RUN_CUSTOM_FLAGS
-p 3306:3306 \
-e MYSQL_PASS=$(MYSQL_PASS)
endef

SHELL_CUSTOM_FLAGS=-P

include $(PWD)/../common/_Makefile

## Run the container in client mode
client:
	echo "Running client of container $(APPNAME) with IP=$(IP)..."
	$(DOCKER) run --rm \
		-it \
		--entrypoint="mysql" \
		$(IMAGE) \
		-uadmin -p$(MYSQL_PASS) -h$(IP)

## Dump database to disk (need DB_NAME)
dump:
ifndef DB_NAME
	$(error DB_NAME is undefined)
else
	echo "Dumping container $(APPNAME) database: $(DB_NAME)..."
	$(DOCKER) run --rm \
		--volumes-from $(APPNAME)_volumes \
		--entrypoint='bash' \
		$(IMAGE) \
		-c "mysqldump -uadmin -p$(MYSQL_PASS) -h$(IP) $(DB_NAME) > /var/lib/$(APPNAME)/backup/$(DATE).sql"
	echo "Backup: /var/lib/$(APPNAME)/backup/$(DATE).sql"
endif


## Restore database from disk (need BACKUP_NAME and DB_NAME)
restore:
ifndef BACKUP_NAME
	$(error BACKUP_NAME is undefined)
else ifndef DB_NAME
	$(error DB_NAME is undefined)
else
	echo "Restoring container $(APPNAME) data from backup $(BACKUP_NAME)..."
	$(DOCKER) run --rm \
		--volumes-from $(APPNAME)_volumes \
		--entrypoint='bash' \
		$(IMAGE) \
		-c "mysql -uadmin -p$(MYSQL_PASS) -h$(IP) $(DB_NAME) < /var/lib/$(APPNAME)/backup/$(BACKUP_NAME)"
endif
