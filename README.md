# Keyclock & Monk
This repository contains Monk.io template to deploy Keyclock either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

# Prerequisites
- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add Instance](https://docs.monk.io/docs/multi-cloud)

#### Make sure monkd is running.
```bash
foo@bar:~$ monk status
daemon: ready
auth: logged in
not connected to cluster
```

## Clone Repository
```bash
git clone https://github.com/monk-io/monk-keyclock
```

## Load Template
```bash
cd monk-keyclock
monk load MANIFEST
```

#### Let's take a look at the themes I have installed.
```bash
foo@bar:~$ monk list monk-keyclock
✔ Got the list
Type      Template                    Repository  Version  Tags
runnable  monk-keyclock/keyclock-db   local       -        -
runnable  monk-keyclock/keyclock-web  local       -        -
group     monk-keyclock/stack         local       -        -
```

## Deploy Stack
```bash
foo@bar:~$ monk run monk-consul/stack
? Select tag to run [local/monk-keyclock/stack] on: mnk
✔ Starting the job: local/monk-keyclock/stack... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% postgres:latest mnk
✔ [================================================] 100% quay.io/keycloak/keycloak:legacy mnk
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ Started local/monk-keyclock/stack

🔩 templates/local/monk-keyclock/stack
 └─🧊 Peer mnk
    ├─🔩 templates/local/monk-keyclock/keyclock-db
    │  └─📦 7794ccb846e06b05793752881bf4c929--keyclock-keyclock-db-postgres
    │     ├─🧩 postgres:latest
    │     ├─💾 /var/lib/monkd/volumes/keyclock/db_data -> /var/lib/postgresql/data
    │     └─🔌 open 13.50.100.228:5432 (0.0.0.0:5432) -> 5432
    └─🔩 templates/local/monk-keyclock/keyclock-web
       └─📦 7e385cfff55cb3888238bc22e2a10542-eyclock-web-keyclock-container
          ├─🧩 quay.io/keycloak/keycloak:legacy
          ├─💾 /var/lib/monkd/volumes/monk-mssql -> /var/opt/mssql
          └─🔌 open 13.50.100.228:8080 (0.0.0.0:8080) -> 8080

💡 You can inspect and manage your above stack with these commands:
	monk logs (-f) local/monk-keyclock/stack - Inspect logs
	monk shell     local/monk-keyclock/stack - Connect to the container's shell
	monk do        local/

```
## Test Web UI

`http://13.50.100.228:8080/auth/`

## Variables
The variables are in `stack.yml` file. You can quickly setup by editing the values here.

| Variable                     	| Description                               	|
|------------------------------	|-------------------------------------------	|
| database_name                 | Keyclock database name, Default: keyclock     |
| database_user                 | Keyclock database user, Default keyclock      |
| database_schema               | Keyclock database schema, Default: public    	|
| database_password             | Keyclock database password, Default: keyclock    	|
| keyclock_user                 | Keyclock web user, Default keyclock          	|
| keyclock_password             | Keyclock web password, Default 8001         	|
| database_port                 | Keyclock database port Default: 5432            	|
| keyclock_port                 | Keyclock web port Default: 8080             	|

## Stop, remove and clean up workloads and templates

```bash
monk purge -x -a
```

