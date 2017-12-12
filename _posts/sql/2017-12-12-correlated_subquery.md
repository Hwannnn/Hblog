---
layout: post
title: 상관서브쿼리와 IN절
published: True
categories: 
- sql
tags:
- database
- mysql
- oracle
- SQL 첫걸음
---


## 상관 서브쿼리

* 상관 서브쿼리란, 서브쿼리 안에 바깥 쪽의 컬럼을 이용한 쿼리로 다음 예제와 같다.

* ```sql
  SELECT s1.no, s1.name
  FROM sample1 s1
  WHERE s1.no = (
  	SELECT s2.no
  	FROM sample2 s2
  	WHERE s1.no = s2.no 
  	);
  ```

  * 이 쿼리는 sample1의 데이터 중에 sample1의 no와 sample2의 no가 같은 데이터를 추출한다.
  * 부모 쿼리의 sample1 테이블의 no가 자식 쿼리의 sample2 테이블의 no와 관련지어 실행되기 때문에 상관 서브쿼리라고 부른다.



### IN 절

* = 연산이 스칼라 값과 비교한다면 IN 절은 집합과 비교한다. 특정 값이 집합에 포함되었는지에 따라 true, false를 반환한다.

* 다음과 같은 테이블이 있다고 하고, IN에 대한 결과가 어떻게 나오는지 보자.

  * | no   | name |
    | ---- | ---- |
    | 1    | NULL |
    | 2    | NULL |
    | 3    | NULL |
    | 4    | NULL |
    | 5    | NULL |

  * ```sql
    SELECT no IN (1, 2) as test
    FROM sample1
    ```

  * | test |
    | ---- |
    | 1    |
    | 1    |
    | 0    |
    | 0    |
    | 0    |

* 결과 값으로 true는 1, false는 0으로 나오는 것을 볼 수 있다.



* 그런데 IN절은 집합 함수와 달리 집합에 NULL이 있다면 무시하지는 않는다. 단, NULL과의 연산이 IS NULL이 아니므로 결과는 NULL 나온다.

  * ```sql
    SELECT no IN (1, NULL) as test
    FROM sample1
    ```

  * | test |
    | ---- |
    | 1    |
    | NULL |
    | NULL |
    | NULL |
    | NULL |



### NOT IN 절

* IN 절과 반대로 포함하지 않는지에 따라 true와 false을 반환한다.

* 마찬가지로 예시를 보자

  * ```sql
    SELECT no NOT IN (1, 2) as test
    FROM sample1
    ```

  * | test |
    | ---- |
    | 0    |
    | 0    |
    | 1    |
    | 1    |
    | 1    |

    ​

  * ```sql
    SELECT no NOT IN (6, 7) as test
    FROM sample1
    ```

  * | test |
    | ---- |
    | 1    |
    | 1    |
    | 1    |
    | 1    |
    | 1    |

    ​

  * ```sql
    SELECT no NOT IN (6, 7, NULL) as test
    FROM sample1
    ```

  * | test |
    | ---- |
    | NULL |
    | NULL |
    | NULL |
    | NULL |
    | NULL |



* 두 번째, 세 번째 예제를 비교해보면, NOT IN절을 이용할 때, 주의해야한다는 것을 볼 수 있다.
* NOT IN 절을 사용하는 집합에 NULL이 포함될 수도 있는지 확인해봐야한다.