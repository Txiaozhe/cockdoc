# build a binary executable file

Build a binary executable file from source code.

For convenience, I will use my own docker image include all environment that build this binary file needed.

Clone all source code from github:
```shell
$ mkdir ~/go/src/cockroachdb
$ cd ~/go/src/cockroachdb
$ git clone https://github.com/cockroachdb/cockroach.git
```
Note that path `~/go/src` is your `GOPATH`

Pull the image:
```shell
$ docker pull txiaozhe/cockroachdb-builder:1.0.3
```

Start docker container:
```shell
$ docker run -it -d -p 26257:26257 \
   -p 8080:8080 \
   -v ~/go/src/cockroachdb/cockroach:/root/go/src/github.com/cockroachdb/cockroach \
   txiaozhe/cockroachdb-builder:1.0.3 \
   bash
```

Enter your container:
```shell
$ docker exec -it CONTAINER_ID bash
```
You can get CONTAINER_ID from `docker ps` if your container was started.

Build:
```shell
$ cd /root/go/src/github.com/cockroachdb/cockroach
$ make build
```

The binary file is generated in `/root/go/src/github.com/cockroachdb/cockroach`.
