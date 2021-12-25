---
title: "ESlint & Prettierrc 설정하기"
description: "코드를 깔끔하게 해주는 친구들을 만나보자"
date: 2021-12-25
update: 2021-12-25
tags:
  - EsLint
  - Prettierrc
  - vscode
series: "vscode settings"
---

## ESlint 설정

### ESLint를 설치

1. vscode Extenstion에서 ESLint 설치
2. npm에서 eslint설치

```shell
$ npm install -g eslint
```

3. package.json 생성

```shell
$ npm init
```

4. .eslintc.js생성

```shell
$ eslint --init
```

> [규칙을 알고 싶다면!](https://eslint.org/docs/rules/)

5. settings.json 수정

- vscode에서 `F1`

6. setting를 치고 입력 후 저장

```json
"eslint.validate": [
        {"language": "javascript"},
        {"language": "html"},
    ],
```

7. .eslintrc.js 파일 수정

```js
    "extends": "eslint:recommended",
  "rules": {
      "indent":[
          "error",
          4
      ],
      "no-unused-vars": 1,
      "no-use-before-define": 1,
      "no-redeclare": 1,
      "no-console":0,
  }
```
