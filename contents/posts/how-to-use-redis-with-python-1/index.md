---
title: "How to use redis with python(1)"
description: "redis를 사용해보자"
date: 2021-12-27
update: 2021-12-27
tags:
  - redis
  - python
series: "How to use redis with python"
---

## Redis

### 무엇을 배울 것인가

- Installing Redis from source and understanding the purpose of the resulting binaries
- Learning a bite-size slice of Redis itself, including its syntax, protocol, and design
- Mastering redis-py while also seeing glimpses of how it implements Redis’ protocol
- Setting up and communicating with an Amazon ElastiCache Redis server instance

### Redis 설치하기

```shell
$ redisurl="http://download.redis.io/redis-stable.tar.gz"
$ curl -s -o redis-stable.tar.gz $redisurl
$ sudo su root
$ mkdir -p /usr/local/lib/
$ chmod a+w /usr/local/lib/
$ tar -C /usr/local/lib/ -xzf redis-stable.tar.gz
$ rm redis-stable.tar.gz
$ cd /usr/local/lib/redis-stable/
$ make && make install
```

#### Redis 버전 체크하기

```shell
$ redis-cli --version
```

### Redis conf 작성하기

```
$ sudo su root
$ mkdir -p /etc/redis/
$ touch /etc/redis/6379.conf
$ vi /etc/redis/6379.conf
```

```
# /etc/redis/6379.conf

port              6379
daemonize         yes
save              60 1
bind              127.0.0.1
tcp-keepalive     300
dbfilename        dump.rdb
dir               ./
rdbcompression    yes
```

### Redis 개념

- client-server 아키텍처를 가지고 있다
- request-response 모델을 사용한다
  - 이로 인해 6379 포트에서 TCP 연결을 할 수 있게 된다
- 많은 클라이언트가 동일한 서버와 통신을 할 수 있다.
  - 각 클라이언트는 서버 응답을 기다리는 소켓에서 읽기를 수행한다.

### Redis 실행하기

```
$ redis-server
```

#### Redis 테스트하기

```
$ redis-cli
redis 127.0.0.1:6379> ping
PONG
redis 127.0.0.1:6379> set mykey somevalue
OK
redis 127.0.0.1:6379> get mykey
"somevalue"
```

### Redis Dictionary

- Redis 데이터베이스는 키:값 쌍을 보유하고 있으며 GET, SET, 및 DEL, 수백 개의 추가 명령 과 같은 명령을 지원한다
- Redis 키는 항상 문자열이다
- 많은 Redis 명령은 Python dict또는 해시 테이블 에서 값을 검색하는 것처럼 일정한 O(1) 시간으로 작동한다

#### SET / GET

```shell
# Redis
127.0.0.1:6379> SET Bahamas Nassau
OK
127.0.0.1:6379> SET Croatia Zagreb
OK
127.0.0.1:6379> GET Croatia
"Zagreb"
127.0.0.1:6379> GET Japan
(nil)
```

```python
>>> capitals = {}
>>> capitals["Bahamas"] = "Nassau"
>>> capitals["Croatia"] = "Zagreb"
>>> capitals.get("Croatia")
'Zagreb'
>>> capitals.get("Japan")  # None
```

#### MSET / MGET

- 여러개 키-값 설정 및 가져오기

```shell
127.0.0.1:6379> MSET Lebanon Beirut Norway Oslo France Paris
OK
127.0.0.1:6379> MGET Lebanon Norway Bahamas
1) "Beirut"
2) "Oslo"
3) "Nassau"
```

#### EXISTS

- 키가 존재하는지 확인하기

```shell
127.0.0.1:6379> EXISTS Norway
(integer) 1
127.0.0.1:6379> EXISTS Sweden
(integer) 0
```

### Redis vs Python

```shell
127.0.0.1:6379> HSET realpython url "https://realpython.com/"
(integer) 1
127.0.0.1:6379> HSET realpython github realpython
(integer) 1
127.0.0.1:6379> HSET realpython fullname "Real Python"
(integer) 1
```

```python
data = {
    "realpython": {
        "url": "https://realpython.com/",
        "github": "realpython",
        "fullname": "Real Python",
    }
}
```

- Redis의 필드는 위의 내부 사전에 있는 각 중첩 키-값 쌍의 Python 키와 유사하다
- Redis 는 해시 구조 자체를 보유하는 최상위 데이터베이스 키에 대한 용어 키 를 예약한다

```shell
127.0.0.1:6379> HMSET pypa url "https://www.pypa.io/" github pypa fullname "Python Packaging Authority"
OK
127.0.0.1:6379> HGETALL pypa
1) "url"
2) "https://www.pypa.io/"
3) "github"
4) "pypa"
5) "fullname"
6) "Python Packaging Authority"
```

#### 종료

```shell
127.0.0.1:6379> FLUSHDB
OK
127.0.0.1:6379> QUIT
```
