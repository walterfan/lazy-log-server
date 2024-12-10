# lazy-log-server

lazy-log-server by lazy rabbit studio.

It's inspired by Boost HTTP server example

## Prerequisites

* cmake

```shell
sudo apt-get update
sudo apt-get install build-essential python3
```

* conan

```shell
pip3 install conan httpie
conan profile detect --force
conan remote update conancenter --url="https://center2.conan.io"
```
## Build

```shell
./build.sh
```


## Usage
1. start server
```shell
./bin/log_server -a 0.0.0.0 -p 3000
```

2. send log to server

```shell
curl -X POST http://localhost:3000/logs \
-H "Content-Type: application/json" \
-d '{
  "time": "2024-09-25T02:50:39.880Z",
  "name": "register_service",
  "method": "post",
  "result": 201,
  "duration_ms": 924,
  "endpoint": "192.168.0.8:8080/portal/api/v1/services/register"
}'
```

or use httpie to send request

```shell
http POST http://localhost:3000/logs time="2024-09-25T02:50:39.880Z" \
name="register_service" method="post" result:=201 duration_ms:=924 \
endpoint="192.168.0.8:8080/portal/api/v1/services/register"

HTTP/1.1 200 OK
Connection: close
Content-Length: 26
Content-Type: text/plain
Server: Beast

{
    "code": 0,
    "desc": "ok"
}
```

3. check log
```shell
tail -10 log/log_server*.log

[2024-12-10 11:53:58.148526] [0x00000001f7d4b240] [info] {"time": "2024-09-25T02:50:39.880Z", "name": "register_service", "method": "post", "result": 201, "duration_ms": 924, "endpoint": "192.168.0.8:8080/portal/api/v1/services/register"}
```

## Todo
* authentication support
* websocket support
