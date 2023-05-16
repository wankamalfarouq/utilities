SHELL := /bin/bash

# maybe setup PHONY

repo :
	git clone git@github.com:setel-engineering/api-auth.git ~/Documents/api-auth & \
	git clone git@github.com:setel-engineering/api-iam.git ~/Documents/api-iam & \
	git clone git@github.com:setel-engineering/api-connect.git ~/Documents/api-connect & \
	git clone git@github.com:setel-engineering/api-portal-connect.git ~/Documents/api-portal-connect & \
	git clone git@github.com:setel-engineering/web-identity.git ~/Documents/web-identity & \
	git clone git@github.com:setel-engineering/web-connect.git ~/Documents/web-connect & \
	git clone git@github.com:setel-engineering/web-sessions.git ~/Documents/web-sessions.git &

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

anti-bug-%:
	curl -X 'POST' \
		'http://localhost:3039/api/iam/$*/policies/api-bug-repellent/endpoints' \
		-H 'accept: */*' \
		-H 'Content-Type: application/json' \
		-d '[ \
	{ \
	"permissions": [ \
	"service_bug_repellent" \
	], \
	"url": "/bug", \
	"method": "GET", \
	"title": "bug repellent", \
	"description": "bug repellent", \
	"permanent": false \
	} \
	]'

exec-%:
	API_BASE_URL='http://localhost:3039' \
	ACCESS_CONTROL_CLI_KEY='local@email.com' \
	ACCESS_CONTROL_CLI_SECRET='stO&ecas39xE!I1#uREF' \
	NAMESPACES=$* \
	npm run start --prefix ~/Documents/Repo/Connect/access-control-config -- -d

connect : repo finch

iam : users-master users-setel-customers users-setel-external-services users-partners-external-services

config : anti-bug-master exec-master
