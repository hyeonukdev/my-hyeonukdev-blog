---
title: "nvm 설치하기"
description: "Install nvm"
date: 2021-12-25
update: 2021-12-25
tags:
  - nvm
  - node.js
series: "Node.js 관련"
---

# What is NVM?

Node Version Manager

- Node.js의 버전을 관리해주는 도구

## How to Install?

### NVM 설치

1. brew로 nvm 설치

```
$ brew install nvm
```

2. dir 생성

```
$ mkdir ~/.nvm
```

3. zshrc 수정

```
$ vi ~/.zshrc
```

4. 변경사항 저장

```
$ source ~/.zshrc
```

### NODE.JS 설치

1. node.js 설치

```
$ nvm install 14.17.4
or
$ nvm install --lts
```

### Check node.js 리스트 확인

1. node.js 리스트 확인

```
$ nvm ls
```

2. 특정 node.js 사용하기

```
$ nvm use <version>
```
