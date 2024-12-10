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
pip3 install conan
conan profile detect --force
conan remote update conancenter --url="https://center2.conan.io"
```
## Build

```shell
./build.sh
```


## Usage
```shell
./bin/log_server -a 0.0.0.0 -p 3000
```

## Todo
* authentication support
* websocket support
