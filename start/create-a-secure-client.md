# create a secure client

Create a directory for certificates and a safe directory for the CA key:

```shell
$ mkdir certs my-safe-directory
```
default certificate directory is (${HOME}/.cockroach-certs) and make sure it is empty

Create the CA (Certificate Authority) certificate and key pair:
```shell
$ cockroach cert create-ca \
--certs-dir=certs \
--ca-key=my-safe-directory/ca.key
```
Create the client certificate and key, in this case for the root user:
```shell
$ cockroach cert create-client \
root \
--certs-dir=certs \
--ca-key=my-safe-directory/ca.key
```
this will generate `client.root.crt` and `client.root.key` in `certs/` dir, make secure communication between the built-in SQL shell and the cluster

Create the node certificate and key:
```shell
$ cockroach cert create-node \
localhost \
$(hostname) \
--certs-dir=certs \
--ca-key=my-safe-directory/ca.key
```
generate `node.crt` and `node.key` in `cert/`, will be used to secure communication between nodes. Typically, you would generate these separately for each node since each node has unique addresses; in this case, however, since all nodes will be running locally, you need to generate only one node certificate and key.

start node
```sh
$ cockroach start \
--certs-dir=certs
```

start sql client
```shell
$ cockroach sql --certs-dir=certs
```

create user and set password
```sql
> CREATE USER txiaozhe WITH PASSWORD '123456';
```
now we can login Admin UI([https://127.0.0.1:8080](https://127.0.0.1:8080)) use this username and password.
