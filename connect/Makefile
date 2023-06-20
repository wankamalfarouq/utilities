SHELL := /bin/bash

repo :
	mkdir -p ~/Documents/Repo/Connect \
	git clone git@github.com:setel-engineering/api-auth.git ~/Documents/Repo/Connect api-auth & \
	git clone git@github.com:setel-engineering/api-iam.git ~/Documents/Repo/Connect/api-iam & \
	git clone git@github.com:setel-engineering/api-connect.git ~/Documents/Repo/Connect/api-connect & \
	git clone git@github.com:setel-engineering/api-portal-connect.git ~/Documents/Repo/Connect/api-portal-connect & \
	git clone git@github.com:setel-engineering/web-identity.git ~/Documents/Repo/Connect/web-identity & \
	git clone git@github.com:setel-engineering/web-connect.git ~/Documents/Repo/Connect/web-connect & \
	git clone git@github.com:setel-engineering/web-sessions.git ~/Documents/Repo/Connect/web-sessions.git &

finch :
	finch run -d --name postgres -p 5432:5432 -e POSTGRES_PASSWORD=postgres postgres & \
	finch run -d --name mongo -p 27017:27017 mongo --replSet rs0 & \
	finch run -d --name redis -p 6379:6379 redis &

users-% :
	curl -X 'POST' \
		'http://localhost:3039/api/iam/$*/users' \
		-H 'accept: application/json' \
		-H 'Content-Type: application/json' \
		-d '{ \
	"password": "stO&ecas39xE!I1#uREF", \
	"identifier": "local@email.com" \
	}'

groups-% :
	curl -X 'POST' \
		'http://localhost:3039/api/iam/$*/admin/groups' \
		-H 'accept: application/json' \
		-H 'Content-Type: application/json' \
		-d '{ \
	"name": "god", \
	"description": "god", \
	"default": true \
	}'

# anti-bug-%:
# 	curl -X 'POST' \
# 		'http://localhost:3039/api/iam/$*/policies/api-bug-repellent/endpoints' \
# 		-H 'accept: */*' \
# 		-H 'Content-Type: application/json' \
# 		-d '[ \
# 	{ \
# 	"permissions": [ \
# 	"service_bug_repellent" \
# 	], \
# 	"url": "/bug", \
# 	"method": "GET", \
# 	"title": "bug repellent", \
# 	"description": "bug repellent", \
# 	"permanent": false \
# 	} \
# 	]'

local-exec-%:
	API_BASE_URL='http://localhost:3039' \
	ACCESS_CONTROL_CLI_KEY='local@email.com' \
	ACCESS_CONTROL_CLI_SECRET='stO&ecas39xE!I1#uREF' \
	NAMESPACES=$* \
	npm run start --prefix ~/Documents/Repo/Connect/access-control-config -- -v

# now how the heck do I encrypt secrets in makefile
dev-validate-%:
	API_BASE_URL='https://api-dev.setel.com' \
	ACCESS_CONTROL_CLI_KEY='local@email.com' \
	ACCESS_CONTROL_CLI_SECRET='<YAe&:^E3CJq;-' \
	NAMESPACES=$* \
	npm run start --prefix ~/Documents/Repo/Connect/access-control-config -- -v

dev-diff-%:
	API_BASE_URL='https://api-dev.setel.com' \
	ACCESS_CONTROL_CLI_KEY='local@email.com' \
	ACCESS_CONTROL_CLI_SECRET='<YAe&:^E3CJq;-' \
	NAMESPACES=$* \
	npm run start --prefix ~/Documents/Repo/Connect/access-control-config -- -p

dev-deploy-%:
	API_BASE_URL='https://api-dev.setel.com' \
	ACCESS_CONTROL_CLI_KEY='local@email.com' \
	ACCESS_CONTROL_CLI_SECRET='<YAe&:^E3CJq;-' \
	NAMESPACES=$* \
	npm run start --prefix ~/Documents/Repo/Connect/access-control-config -- -d

connect : repo finch

iam : users-master users-setel-customers users-setel-external-services users-partners-external-services

.PHONY : repo finch connect iam config
