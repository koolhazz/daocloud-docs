---
title: 持续集成的结构和写法
taxonomy:
    category:
        - docs
---

<!-- reviewed by fiona -->

<!-- http://help.daocloud.io/features/continuous-integration/index.html 确定这些功能是否还有效-->

DaoCloud CI 使用基于 Docker 的容器技术来运行您的测试任务，确保您的任务和其他租户的任务完全隔离并互不干扰。

## 入门

您可以通过在代码根目录放置一个 `daocloud.yml` 文件来配置您的测试任务。

```
version: "2.0"
test:
    image: daocloud/ci-golang:1.4

    services:
        - mongodb
        - mysql
        - redis

    env:
        - MYENV = "hello"

    install:
        - echo $MYENV
        - echo "This is an install segment"
        - echo "Here, we usually run scripts to setup a base environment"
        - echo "For customized base image, you need to install git here unless you have git installed in your base image"
        - echo "e.g., apt-get install -y git-core"

    before_script:
        - echo $MYENV
        - echo "This is an before_script segment"
        - echo "Here, we usually run scripts to prepare our test"

    script:
        - echo $MYENV
        - echo "This is an script segment"
        - echo "Run test cases here"
        - echo ""
        - echo "Below shows how to use services, mongodb/mysql/redis are the hostnames of services"
        - ping -c 2 mongodb
        - ping -c 2 mysql
        - ping -c 2 redis
```

## 执行步骤

1. 克隆源代码，切换到对应的提交。
2. 挂载代码到测试环境。
3. 设置环境变量。
4. 执行 `install` 脚本。
5. 执行 `before_script` 脚本。
6. 执行 `script` 脚本。

## 实现细节

DaoCloud 支持**自定义的测试镜像**，让您更方便地配置个性化的测试环境。

为了方便您准备测试环境，我们还提供了数据储存服务：

- MySQL (5.5)
- Redis (2.8)
- MongoDB (2.6)
- RabbitMQ (3.6)
- PostgreSQL (9.1)

### 指定测试镜像

测试镜像代表了您的测试任务运行的环境。

```
＃ The build image you want to use; you can select one from DaoCloud support list.
image: daocloud/ci-golang:1.4
```

您可以从下面的列表（DaoCloud 准备好的镜像）中选择您希望的运行环境，也可以使用**自定义的镜像**：

- daocloud/ci-python:2.6
- daocloud/ci-python:2.7
- daocloud/ci-python:3.1
- daocloud/ci-python:3.2
- daocloud/ci-python:3.3
- daocloud/ci-python:3.4
- daocloud/ci-golang:1.0
- daocloud/ci-golang:1.1
- daocloud/ci-golang:1.2
- daocloud/ci-golang:1.3
- daocloud/ci-golang:1.4
- daocloud/ci-golang:1.5.1
- daocloud/ci-golang:1.5
- daocloud/ci-golang:1.6
- daocloud/ci-node:0.6
- daocloud/ci-node:0.8
- daocloud/ci-node:0.10
- daocloud/ci-node:0.12
- daocloud/ci-php:5.5
- daocloud/ci-php:5.6
- daocloud/ci-ruby:1.8.7
- daocloud/ci-ruby:1.9.3
- daocloud/ci-ruby:2.0.0
- daocloud/ci-ruby:2.1.2
- daocloud/ci-ruby:2.1.4
- daocloud/ci-ruby:2.1.6
- daocloud/ci-ruby:2.2.0
- daocloud/ci-ruby:2.2.2
- daocloud/ci-java:openjdk6
- daocloud/ci-java:openjdk7
- daocloud/ci-java:oraclejdk6
- daocloud/ci-java:oraclejdk7
- daocloud/ci-java:oraclejdk8
- daocloud/ci-java:oraclejdk9
- daocloud/ci-gcc:4.4
- daocloud/ci-gcc:4.6
- daocloud/ci-gcc:4.7
- daocloud/ci-gcc:4.8
- daocloud/ci-gcc:4.9
- daocloud/ci-gcc:5

当然您可以使用其他公开镜像，比如：

- userxxx/python:3.0
- useryyy/golang:1.1
- daocloud.io/python:3.5

### 配置测试服务

您可以指定测试任务运行时依赖的服务，指定服务后，测试任务可以用 `docker link` 一致的方式去访问它。比如您指定要使用 `mysql`，那么您可以使用 `mysql` 作为主机名来访问服务节点，或者通过环境变量 `MYSQL_PORT_3306_TCP_ADDR` 来访问。

```
＃ List of services you want to use during your test.
services:
  - mysql
  - mongodb
```

目前 DaoCloud CI 支持的服务及访问方式如下表所示（该列表正在不断完善中）：

#### MySQL

Version：MySQL 5.5

Docker Link Alias: mysql

Host: mysql

Port: 3306

UserName: root

Password: 不设密码

Default Instance: test

环境变量：

- MYSQL_PORT = tcp://1.2.3.4:3306
- MYSQL_PORT_3306_TCP_ADDR = 1.2.3.4
- MYSQL_PORT_3306_TCP = tcp://1.2.3.4:3306
- MYSQL_PORT_3306_TCP_PROTO = tcp
- MYSQL_PORT_3306_TCP_PORT = 3306

#### Redis

Version：Redis 2.8

Docker Link Alias: redis

Host: redis

Port: 6379

环境变量：

- REDIS_PORT = tcp://1.2.3.4:6379
- REDIS_PORT_6379_TCP_ADDR = 1.2.3.4
- REDIS_PORT_6379_TCP = tcp://1.2.3.4:6379
- REDIS_PORT_6379_TCP_PROTO = tcp
- REDIS_PORT_6379_TCP_PORT = 6379

#### MongoDB

Version：MongoDB 2.6

Docker Link Alias: mongodb

Host: mongodb

Port: 27017

环境变量：

- MONGODB_PORT = tcp://1.2.3.4:27017
- MONGODB_PORT_27017_TCP_ADDR = 1.2.3.4
- MONGODB_PORT_27017_TCP = tcp://1.2.3.4:27017
- MONGODB_PORT_27017_TCP_PROTO = tcp
- MONGODB_PORT_27017_TCP_PORT = 27017

#### RabbitMQ

Version：RabbitMQ 3.6

Docker Link Alias: rabbitmq

Host: rabbitmq

Port: 5672

环境变量：

- RABBITMQ_PORT_5672_TCP_ADDR = 1.2.3.4
- RABBITMQ_PORT_5672_TCP = tcp://1.2.3.4:5672
- RABBITMQ_PORT_5672_TCP_PROTO = tcp 
- RABBITMQ_PORT_5672_TCP_PORT = 5672

#### PostgreSQL

Version：PostgreSQL 9.1

Docker Link Alias: postgres

Host: postgres

Port: 5432

Username: postgres
Password: 无

环境变量：

- POSTGRES_PORT_5432_TCP_ADDR=1.2.3.4
- POSTGRES_PORT_5432_TCP_PORT=5432
- POSTGRES_PORT_5432_TCP_PROTO=tcp
- POSTGRES_PORT=tcp://1.2.3.4:5432
- POSTGRES_PORT_5432_TCP=tcp://1.2.3.4:5432

### 设置测试环境变量

```
＃ Environment variables
env:
  - GOPATH = /go
  - MY_ENV = abc
```

### 指定镜像初始化脚本

您可以通过 `install` 标签来指定您对镜像的初始化脚本。

```
＃ Scripts you want to run before cloning the repo
install:
  - sudo apt-get update
  - sudo apt-get -y install bzr
```

### 指定测试准备脚本

您可以通过 `before_script` 标签来为您的测试做准备。

```
＃ Scripts you want to run before your test
before_script:
  - go get github.com/onsi/ginkgo
  - go get labix.org/v2/mgo
```

### 指定测试脚本

您可以通过 `script` 标签来指定您的运行的测试任务。

```
＃ Scripts you want to run for the test
script:
  - go test your-code
```
