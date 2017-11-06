---
layout: post
title: SQL 집계함수
published: True
categories: 
- mysql
tags:
- database
- mysql
- oracle
- SQL 첫걸음
---


## 집계함수
* 대표적인 집계함수: COUNT, SUM, AVG, MIN, MAX
* 인수로 집합을 다루므로 집합함수라고도 부른다.

## NULL 값 처리
* 집계함수에서는 집함안에 NULL 값이 있을 경우 이를 제외하고 처리한다.
* 이를 확인하기 위해 다음과 같은 테이블이 있다고 치자.

| no        | name      | quantity  |
| :-------- | :-------- | :-------- |
| 1         | A         | 1         |
| 2         | A         | 2         |
| 3         | B         | 10        |
| 4         | C         | 3         |
| 5         | NULL      | NULL      |


* 그리고 다음과 같은 쿼리를 작성하여, name 열에서 NULL 값이 제외되는지 확인해보자.
: SELECT COUNT(no), COUNT(name) FROM sample

| COUNT(no) | COUNT(name) |
| :-------- | :--------   |
| 5         | 4           |

<br/>

* 앞서 말한바와 같이 name 열의 경우에는 NULL 값이 제외한 4개의 행의 수만 카운트되었다.
* COUNT 뿐만 아니라 다른 집계함수의 경우에도 NULL에 대한 처리는 같다.
* 그리고 COUNT(*)의 경우에는 모든 열의 행 수를 카운트하기 때문에, NULL 값이 중간에 포함되어 있어도 제외되지 않는다.
