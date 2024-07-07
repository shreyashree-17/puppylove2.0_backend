[PuppyLove Backend Repo](https://github.com/shreyashree-17/puppylove2.0_frontend)
# Puppylove2.0 Backend

>Commands for running docker container are in `Docker.md`

## Me-My Encryption

Encryption used in Puppylove2.0 is explained below.

### How to setup for development

Clone the repo from the github link & use command `go mod tidy` to install all dependencies (make sure you've installed go).

Install(if not already) & Run the postgres server(refer [link](https://www.postgresql.org/download/)) on your device (depending the OS you're using).

Depending on the authentication details you've set up modify the `.env` file(refer envformat.txt) by filling in details of the postgres server.

> Format of the .env file -- Don't forget to disable SSL if not already.

```
POSTGRES_HOST = localhost
POSTGRES_PORT = 5433
POSTGRES_PASSWORD = admin
POSTGRES_DB = puppylove
POSTGRES_USER = pratham
CFG_ADMIN_PASS = "something"

ADMIN_ID = "admin"
ADMIN_PASS = "admin"

USER_JWT_SIGNING_KEY = "something"
HEART_JWT_SIGNING_KEY = "something2"
DOMAIN = "localhost"

EMAIL_ID = "hello@iitk.ac.in"
EMAIL_PASS = "hello"
``` 

Build the app by using `go mod build` & then run `./me-my_encryption` (the generated file).
OR you may directly run the main.go file as well.

Server should be up & listening at port 8080.

### Deploying the backend and database

For deployment we have chosen to go with a Kubernetes cluster as it is easily scalable and crashes of the backend, do not affect the state of the database.
For deploying a server, kindly see `DEPLOYMENT.md`.

## About the Algorithm (I'll try eplaining better when I get time)

let the key to be sent here be 
k = my_roll_me_roll_rand (eg. `210667_21xxxx_&89h9hKJbx`)
```mermaid
sequenceDiagram
Me ->> Server: SHA(k1), enc(SHA(k1))<br/> SHA(k2), enc(SHA(k2))...
Note right of Server: On login
My-->>Server: Give me all enc 
Server-->>My: enc1, enc2, ...
My-->>Server: enc1 is mine
Server-->>My: How do I verify ?
My-->>Server: Here is SHA I got<br/> on decoding,
Note left of Server: SHA & enc match.<br/> So, assign the pair to user.
Note left of My: Now when "my" send his/her<br/>hearts, assigned pairs are also sent.<br/> Also, "my" gets a token for 10min.<br/>To send claimed heart again <br/>if not sent already. (quick fix for clash)
My->>Server: SHA(l1), enc(SHA(l1))<br/> SHA(l2), enc(SHA(l2))...<br/> + <br/> SHA(k1), enc(SHA(k1)) <br/>-enc with pubkey of receiver.
Server-->Me: Syncing data with Me...
Me->>Server: I got my heart back.
Server->>Me: How do I verify ?
Me->>Server: Here is my k(210667_21xxxx_&89h9hKJbx).<br/>Verify it with SHA.
Me->My: Matched
 ```

## Populate Users in Postgresql
In test_suite/info.json replace *adminID* with your **SUPERUSER** of postgres and *adminPASS* with its **password**. <br />
And don't make PR with your credentials.<br />
<!-- -->
Install the dependencies for Test Suite:
```bash
pip install -r test_suite/requirements.txt
```
To Populate all the Users
```bash
cd test_suite
python populate.py
```
To register all the users
```bash
cd test_suite
python register_all_users.py {password} {database} {host} {port}
```
Where:
* password: Password for all the users
* database: Database name (Default = puppylove)
* host: Default = 127.0.0.1
* port: Default = 5432
