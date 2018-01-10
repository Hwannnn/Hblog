---
layout: post
title: 문자열 데이터 저장공간 계산
published: True
categories: 
- sql
tags:
- database
- mysql
- oracle
- SQL 첫걸음
---



### 인코딩에 따른 바이트 저장공간 크기
* 컴퓨터에 저장되는 문자열 데이터는 결국 수치로 저장된다.
* SQL도 문자열 데이터를 수치로 저장하는데, 인코딩 방식에 따라 저장공간의 크기가 달라진다.
* 즉, 문자열을 수치화하는 과정(인코딩 과정)에 따라 저장공간의 크기가 달라진다.



* UTF-8은 아스키 문자는 1바이트 한글은 3바이트를 가지고, euc-kr은 아스키문자는 바이트, 한글은 2바이트의 저장공간을 가진다

  * charater_length(char_length)는 문자수를 계산하는 연산이지만, octet_length는 바이트 수를 계산하는 연산 이므로 인코딩방식을 주의하여 사용해야합니다.

  * ```mysql
    SELECT char_ex
         , char_length(char_ex)
         , octet_length(char_ex)
      FROM utf8_table
    ```

  * | char_ex | char_length(char_ex) | octet_length(char_ex) |
    | ------- | -------------------- | --------------------- |
    | 문자열테스트  | 6                    | 18                    |

  ​

  * ```mysql
    SELECT char_ex
         , char_length(char_ex)
         , octet_length(char_ex)
      FROM euckr_table
    ```

  * | char_ex | char_length(char_ex) | octet_length(char_ex) |
    | ------- | -------------------- | --------------------- |
    | 문자열테스트  | 6                    | 12                    |



### 테이블 인코딩, 컬럼 인코딩
* 처음에 create table를 이용해 테이블을 만들면 utf8이 디폴트여서 euckr로 바꾸고 테스트를 진행했는데, 한글 1자에 3바이트로 저장되었다. 
* 인코딩을 변경한후 새로운 컬럼을 추가해서 시도해보니, 새로 추가된 컬럼은 제대로 2바이트로 저장되었다.


* 결론적으로 테이블 인코딩말고 컬럼마다도 인코딩을 설정할 수 있었다.



* 처음 테이블을 만들 때 같이 생성되는 컬럼들의 인코딩이 테이블의 인코딩을 따라 간다.  

  - 테이블의 인코딩을 euckr로 변경한 후 alter table add 컬럼을 하여 새로운 컬럼을 넣으면, 그 새로운 컬럼은 현재 테이블의 인코딩을 따라가기에 euckr로 추가된다.

  ​

* 테이블의 인코딩을 바꿔도 컬럼의 인코딩은 그대로 남아있어서 위와같은 결과가 나왔던 거였다.

```mysql
CREATE TABLE sample (
	a char(10)
) DEFAULT CHARACTER SET = UTF8;

ALTER TABLE sample CHARACTER SET EUCKR;
ALTER TABLE sample ADD b char(10);

INSERT INTO sample(a, b) VALUES('가', '가')
```

```mysql
SELECT a
     , char_length(a)
     , octet_length(a)
     , b
     , char_length(b)
     , octet_length(b)
  FROM sample
```

| a    | char_length(a) | octet_length(a) | b    | char_length(b) | octet_length(b) |
| ---- | -------------- | --------------- | ---- | -------------- | --------------- |
| 가    | 1              | 3               | 가    | 1              | 2               |



- 이처럼 기존의 테이블의 인코딩 정보를 변경할 때, 기존의 컬럼들의 인코딩 정보까지 모두 변경하기 위해서는 다음과 같이 CONVERT TO 를 추가해준다.

* ```mysql
  ALTER TABLE aa CONVERT TO CHARACTER SET euckr
  ```



### mysql 문자열 저장 공간
* table을 정의할때 varchar(60)처럼 (n)으로 열 타입의 크기를 정하는데, mysql 4.1 이전에는 n이 바이트였다면, mysql 4.1 이후부터는 문자 수다.
* 즉, mysql4.1 이후부터 varcher(60)이면 아스키 문자든 한글이든 60자만큼 입력가능하다는 의미이다.