SHELL := /bin/bash

define SECRET_DEV
$$ANSIBLE_VAULT;1.1;AES256
39333433386639333930653134666630383166346333313738376637323939393630323466366662
3238643262383865316466623764666263316563353536310a393362663336633565336662323162
37306439313038623434646430346536663233303234373836353461306633663861343065613334
3164386636663739340a636230343934633139326335326162326435323236663139313238663330
3134
endef

export SECRET_DEV

all : users interfaces

repo :
	mkdir -p ~/Documents/Repo/Connect \
	git clone setel:setel-engineering/api-auth.git ~/Documents/Repo/Connect api-auth & \
	git clone setel:setel-engineering/api-iam.git ~/Documents/Repo/Connect/api-iam & \
	git clone setel:setel-engineering/api-connect.git ~/Documents/Repo/Connect/api-connect & \
	git clone setel:setel-engineering/api-portal-connect.git ~/Documents/Repo/Connect/api-portal-connect & \
	git clone setel:setel-engineering/web-identity.git ~/Documents/Repo/Connect/web-identity & \
	git clone setel:setel-engineering/web-connect.git ~/Documents/Repo/Connect/web-connect & \
	git clone setel:setel-engineering/web-sessions.git ~/Documents/Repo/Connect/web-sessions.git &

# finch :
# 	finch run -d --name postgres -p 5432:5432 -e POSTGRES_PASSWORD=postgres postgres & \
# 	finch run -d --name mongo -p 27017:27017 mongo --replSet rs0 & \
# 	finch run -d --name redis -p 6379:6379 redis &

users-% :
	curl -X 'POST' \
		'http://localhost:3039/api/iam/$*/users' \
		-H 'accept: application/json' \
		-H 'Content-Type: application/json' \
		-d '{ \
	"password": "stO&ecas39xE!I1#uREF", \
	"identifier": "local@email.com" \
	}'

create-users-% :
	for i in `seq $(min) $(max)`; do \
		if [[ $$(($$i % 100)) == 0 ]]; then \
			sleep 3; \
		fi; \
		curl -X 'POST' \
		'http://localhost:3039/api/iam/$*/users' \
		-H 'accept: application/json' \
		-H 'Content-Type: application/json' \
		-d '{ \
		"password": "stO&ecas39xE!I1#uREF", \
		"identifier": "dummy'"$$i"'" \
		}'& \
	done

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

interfaces-% :
	curl -X 'POST' \
		'http://localhost:3039/api/iam/$*/admin/interfaces' \
		 -H 'accept: application/json' \
		 -H 'Content-Type: application/json' \
		 -d '{ \
	"identifier": "default", \
	"name": "Default", \
	"type": "default", \
	"sessionPoolId": null \
}'

local-validate-% :
	API_BASE_URL='http://localhost:3039' \
	ACCESS_CONTROL_CLI_KEY='local@email.com' \
	ACCESS_CONTROL_CLI_SECRET='stO&ecas39xE!I1#uREF' \
	NAMESPACES=$* \
	npm run start --prefix ~/Documents/Repo/Connect/access-control-config -- -v

local-diff-% :
	API_BASE_URL='http://localhost:3039' \
	ACCESS_CONTROL_CLI_KEY='local@email.com' \
	ACCESS_CONTROL_CLI_SECRET='stO&ecas39xE!I1#uREF' \
	NAMESPACES=$* \
	npm run start --prefix ~/Documents/Repo/Connect/access-control-config -- -p

local-deploy-% :
	API_BASE_URL='http://localhost:3039' \
	ACCESS_CONTROL_CLI_KEY='local@email.com' \
	ACCESS_CONTROL_CLI_SECRET='stO&ecas39xE!I1#uREF' \
	NAMESPACES=$* \
	npm run start --prefix ~/Documents/Repo/Connect/access-control-config -- -d

dev-validate-% :
	API_BASE_URL='https://api-dev.setel.com' \
	ACCESS_CONTROL_CLI_KEY='local@email.com' \
	ACCESS_CONTROL_CLI_SECRET=$$(echo "$$SECRET_DEV" | ansible-vault decrypt --vault-password-file ~/.ssh/setel) \
	NAMESPACES=$* \
	npm run start --prefix ~/Documents/Repo/Connect/access-control-config -- -v

dev-diff-% :
	API_BASE_URL='https://api-dev.setel.com' \
	ACCESS_CONTROL_CLI_KEY='local@email.com' \
	ACCESS_CONTROL_CLI_SECRET=$$(echo "$$SECRET_DEV" | ansible-vault decrypt --vault-password-file ~/.ssh/setel) \
	NAMESPACES=$* \
	npm run start --prefix ~/Documents/Repo/Connect/access-control-config -- -p

dev-deploy-% :
	API_BASE_URL='https://api-dev.setel.com' \
	ACCESS_CONTROL_CLI_KEY='local@email.com' \
	ACCESS_CONTROL_CLI_SECRET=$$(echo "$$SECRET_DEV" | ansible-vault decrypt --vault-password-file ~/.ssh/setel) \
	NAMESPACES=$* \
	npm run start --prefix ~/Documents/Repo/Connect/access-control-config -- -d

users : users-master users-setel-customers users-setel-external-services users-partners-external-services

interfaces : interfaces-master interfaces-setel-customers interfaces-setel-external-services interfaces-partners-external-services

.PHONY : repo finch connect iam config users groups interfaces all
