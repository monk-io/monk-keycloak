# Keycloak & Monk

This repository contains Monk.io template to deploy Keycloak either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

## Prerequisites

- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add Instance](https://docs.monk.io/docs/multi-cloud)

## Make sure monkd is running

```bash
foo@bar:~$ monk status
daemon: ready
auth: logged in
not connected to cluster
```

## Clone Repository

```bash
git clone https://github.com/monk-io/keycloak
```

## Load Template

```bash
cd keycloak
monk load MANIFEST
```

```bash
foo@bar:~$ monk list keycloak
✔ Got the list
Type      Template                    Repository  Version  Tags
runnable  keycloak/keycloak-db   local       -        -
runnable  keycloak/keycloak-web  local       -        -
group     keycloak/stack         local       -        -
```

## Deploy Stack

```bash
foo@bar:~$ monk run monk-consul/stack
? Select tag to run [local/keycloak/stack] on: mnk
✔ Starting the job: local/keycloak/stack... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% postgres:latest mnk
✔ [================================================] 100% quay.io/keycloak/keycloak:legacy mnk
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ Started local/keycloak/stack

🔩 templates/local/keycloak/stack
 └─🧊 Peer mnk
    ├─🔩 templates/local/keycloak/keycloak-db
    │  └─📦 7794ccb846e06b05793752881bf4c929--keycloak-keycloak-db-postgres
    │     ├─🧩 postgres:latest
    │     ├─💾 /var/lib/monkd/volumes/keycloak/db_data -> /var/lib/postgresql/data
    │     └─🔌 open 13.50.100.228:5432 (0.0.0.0:5432) -> 5432
    └─🔩 templates/local/keycloak/keycloak-web
       └─📦 7e385cfff55cb3888238bc22e2a10542-eyclock-web-keycloak-container
          ├─🧩 quay.io/keycloak/keycloak:legacy
          ├─💾 /var/lib/monkd/volumes/monk-mssql -> /var/opt/mssql
          └─🔌 open 13.50.100.228:8080 (0.0.0.0:8080) -> 8080

💡 You can inspect and manage your above stack with these commands:
 monk logs (-f) local/keycloak/stack - Inspect logs
 monk shell     local/keycloak/stack - Connect to the container's shell
 monk do        local/

```

## Test Web UI

`http://13.50.100.228:8080/auth/`

## Variables

The variables are in `stack.yml` file. You can quickly setup by editing the values here.

| Variable          | Description                                   |
| ----------------- | --------------------------------------------- |
| database_name     | Keycloak database name, Default: keycloak     |
| database_user     | Keycloak database user, Default keycloak      |
| database_schema   | Keycloak database schema, Default: public     |
| database_password | Keycloak database password, Default: keycloak |
| keycloak_user     | Keycloak web user, Default keycloak           |
| keycloak_password | Keycloak web password, Default 8001           |
| database_port     | Keycloak database port Default: 5432          |
| keycloak_port     | Keycloak web port Default: 8080               |

## Stop, remove and clean up workloads and templates

```bash
monk purge -x -a
```
