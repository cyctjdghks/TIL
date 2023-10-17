# MySQL에서 JSON 타입 컬럼 사용하기

## MySQL에서 JSON 타입을 사용하는 이유

### 1. 유연한 데이터 저장:

JSON 타입 컬럼을 사용하면 구조화된 데이터 형식을 유연하게 저장할 수 있습니다. JSON은 키-값 쌍의 형태로 데이터를 저장하며, 데이터 구조를 자유롭게 조정할 수 있습니다. 이는 데이터 모델이 변화하거나 다양한 유형의 데이터를 저장해야 하는 경우에 유용합니다.

### 2. 복잡한 데이터 다루기:

JSON 타입 컬럼을 사용하면 복잡한 데이터를 쉽게 저장하고 쿼리할 수 있습니다. JSON 데이터는 중첩된 구조를 가질 수 있으며, 배열과 객체를 포함할 수 있어 다양한 데이터 모델에 대응할 수 있습니다.

### 3. 응용 프로그램과의 호환성:

많은 웹 및 모바일 응용 프로그램은 JSON 형식의 데이터를 주고받습니다. JSON 타입 컬럼을 사용하면 데이터베이스와 응용 프로그램 간의 데이터 교환을 간소화할 수 있습니다. 데이터베이스에서 JSON 형식으로 저장하고, 응용 프로그램에서 JSON 형식으로 직접 활용할 수 있습니다.

### 4. 데이터 직렬화 및 역직렬화:

JSON 데이터는 일반적으로 문자열로 저장되며, 이를 직렬화(serialize)하거나 역직렬화(deserialize)하는 작업이 쉽습니다. 이로써 데이터를 저장 및 검색하는 데 유리한 데이터 형식으로 활용할 수 있습니다.

### 5. 스키마 변경 용이성:

관계형 데이터베이스의 테이블 스키마를 변경하는 것은 복잡하고 비용이 많이 드는 일이지만, JSON 타입 컬럼을 사용하면 스키마의 일부를 유지하면서도 새로운 데이터 필드를 추가하거나 제거하는 것이 간단합니다.

### 6. NoSQL 및 관계형 데이터의 중간 지점:

JSON 타입 컬럼을 사용하면 관계형 데이터베이스와 NoSQL 데이터베이스 간의 중간 지점 역할을 할 수 있습니다. 복잡한 JSON 데이터를 저장하면서도 SQL 쿼리를 사용하여 데이터에 접근할 수 있습니다.

### 7. 빠른 개발 및 프로토타이핑:

JSON 타입 컬럼을 사용하면 데이터베이스의 스키마를 자주 변경하지 않고도 빠르게 개발 및 프로토타이핑할 수 있습니다. 새로운 데이터 필드를 추가하거나 변경하는 데 훨씬 적은 노력이 필요합니다.

#### 따라서 MySQL에서 5.7 버전부터 JSON Column을 지원하고 있습니다.


## 1. 테이블 생성

```sql
CREATE TABLE `jsontable` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(45) NULL,
  `jsoncol` JSON NULL,
  PRIMARY KEY (`id`)
);
```

## 2. JSON 데이터 입력

- JSON 문자열 입력
```sql
INSERT INTO `jsontable`(`name`, `jsoncol`) VALUES ("json_string", '{"a": "A", "b":"B"}');
```

- JSON 객체 입력
```sql
INSERT INTO `jsontable`(`name`, `jsoncol`) VALUES ("json_object", json_object("a", "A", "b", "B"));
```

- JSON 객체에 배열 입력
```sql
INSERT INTO `jsontable`(`name`, `jsoncol`) VALUES ("json_object with list", json_object("a", "[1,2,3]", "b", "B"));
```

- JSON 객체에 JSON 배열 입력
```sql
INSERT INTO `jsontable`(`name`, `jsoncol`) VALUES ("json_object with json_array", json_object("a", JSON_ARRAY(1,2,3), "b", "B"));
```

## 3. JSON 데이터 조회

### 일반적인 읽기
```sql
SELECT * FROM jsontable;

SELECT id, name, jsoncol FROM jsontable;
```
**결과 예시:**
```
+----+-----------------------------+------------------------------------+
| id | name                        | jsoncol                            |
+----+-----------------------------+------------------------------------+
|  1 | json_string                 | {"a": "A", "b": "B"}               |
|  2 | json_object                 | {"a": "A", "b": "B"}               |
|  3 | json_object with list       | {"a": "[1,2,3]", "b": "B"}         |
|  4 | json_object with json_array | {"a": [1, 2, 3], "b": "B"}         |
|  5 | json_object with list       | {"test": "TEST", "test2": "TEST2"} |
|  6 | json_string                 | {"a": "가나다라", "b": "B"}        |
|  7 | json_string                 | {"a": "나다라", "b": "B"}          |
|  8 | json_string                 | {"a": "다라", "b": "B"}            |
|  9 | json_string                 | {"a": "라", "b": "B"}              |
| 10 | json_string                 | {"a": "마바사", "b": "B"}          |
| 11 | json_string                 | {"a": "고요요", "b": "B"}          |
| 12 | json_string                 | {"a": "1235", "b": "B"}            |
| 13 | json_string                 | {"a": "6542", "b": "B"}            |
+----+-----------------------------+------------------------------------+
```

### 특정 키 값만 불러오기
```sql
SELECT id, name, json_extract(jsoncol, '$.a') FROM jsontable;

SELECT id, name, jsoncol->'$.a' FROM jsontable;
```
**결과 예시:**
```
+----+-----------------------------+----------------+
| id | name                        | jsoncol->'$.a' |
+----+-----------------------------+----------------+
|  1 | json_string                 | "A"            |
|  2 | json_object                 | "A"            |
|  3 | json_object with list       | "[1,2,3]"      |
|  4 | json_object with json_array | [1, 2, 3]      |
|  5 | json_object with list       | NULL           |
|  6 | json_string                 | "가나다라"     |
|  7 | json_string                 | "나다라"       |
|  8 | json_string                 | "다라"         |
|  9 | json_string                 | "라"           |
| 10 | json_string                 | "마바사"       |
| 11 | json_string                 | "고요요"       |
| 12 | json_string                 | "1235"         |
| 13 | json_string                 | "6542"         |
+----+-----------------------------+----------------+
```

### JSON 값에 따옴표(") 없애기
```sql
SELECT id, name, JSON_UNQUOTE(JSON_EXTRACT(jsoncol, '$.a')) FROM jsontable;
```
**결과 예시:**
```
+----+-----------------------------+--------------------------------------------+
| id | name                        | JSON_UNQUOTE(JSON_EXTRACT(jsoncol, '$.a')) |
+----+-----------------------------+--------------------------------------------+
|  1 | json_string                 | A                                          |
|  2 | json_object                 | A                                          |
|  3 | json_object with list       | [1,2,3]                                    |
|  4 | json_object with json_array | [1, 2, 3]                                  |
|  5 | json_object with list       | NULL                                       |
|  6 | json_string                 | 가나다라                                   |
|  7 | json_string                 | 나다라                                     |
|  8 | json_string                 | 다라                                       |
|  9 | json_string                 | 라                                         |
| 10 | json_string                 | 마바사                                     |
| 11 | json_string                 | 고요요                                     |
| 12 | json_string                 | 1235                                       |
| 13 | json_string                 | 6542                                       |
+----+-----------------------------+--------------------------------------------+
```

### 조건식 사용
```sql
SELECT id, name, json_extract(jsoncol, '$.a') FROM jsontable WHERE json_extract(jsoncol, '$.a') = "A";
```
**결과 예시:**
```
+----+-------------+------------------------------+
| id | name        | json_extract(jsoncol, '$.a') |
+----+-------------+------------------------------+
|  1 | json_string | "A"                          |
|  2 | json_object | "A"                          |
+----+-------------+------------------------------+
```

### 정렬
```sql
SELECT id, name, JSON_UNQUOTE(JSON_EXTRACT(jsoncol, '$.a')) AS json FROM jsontable ORDER BY json DESC;
```
**결과 예시:**
```
+----+-----------------------------+-----------+
| id | name                        | json      |
+----+-----------------------------+-----------+
| 10 | json_string                 | 마바사    |
|  9 | json_string                 | 라        |
|  8 | json_string                 | 다라      |
|  7 | json_string                 | 나다라    |
| 11 | json_string                 | 고요요    |
|  6 | json_string                 | 가나다라  |
|  3 | json_object with list       | [1,2,3]   |
|  4 | json_object with json_array | [1, 2, 3] |
|  1 | json_string                 | A         |
|  2 | json_object                 | A         |
| 13 | json_string                 | 6542      |
| 12 | json_string                 | 1235      |
|  5 | json_object with list       | NULL      |
+----+-----------------------------+-----------+
```

## 4. JSON 데이터 변경

### JSON_REPLACE 사용법
1) JSON 데이터의 'a' 키 값을 모두 'B'로 바꾸기 => SELECT 문에 사용 시 원본 데이터 변경X
```sql
SELECT id, name, JSON_REPLACE(jsoncol, '$.a', "B") FROM jsontable;
```
**결과 예시:**
```
+----+-----------------------------+------------------------------------+
| id | name                        | JSON_REPLACE(jsoncol, '$.a', "B")  |
+----+-----------------------------+------------------------------------+
|  1 | json_string                 | {"a": "B", "b": "B"}               |
|  2 | json_object                 | {"a": "B", "b": "B"}               |
|  3 | json_object with list       | {"a": "B", "b": "B"}               |
|  4 | json_object with json_array | {"a": "B", "b": "B"}               |
|  5 | json_object with list       | {"test": "TEST", "test2": "TEST2"} |
|  6 | json_string                 | {"a": "B", "b": "B"}               |
|  7 | json_string                 | {"a": "B", "b": "B"}               |
|  8 | json_string                 | {"a": "B", "b": "B"}               |
|  9 | json_string                 | {"a": "B", "b": "B"}               |
| 10 | json_string                 | {"a": "B", "b": "B"}               |
| 11 | json_string                 | {"a": "B", "b": "B"}               |
| 12 | json_string                 | {"a": "B", "b": "B"}               |
| 13 | json_string                 | {"a": "B", "b": "B"}               |
+----+-----------------------------+------------------------------------+
```

2) JSON 데이터 수정
```sql
UPDATE jsontable SET jsoncol = JSON_REPLACE(jsoncol, '$.a', "B") WHERE id = 6;

UPDATE jsontable SET jsoncol = JSON_SET(jsoncol, '$.a', "B") WHERE id = 6;
```
**결과 예시:**
```
+----+-----------------------------+----------------------------+
| id | name                        | jsoncol                    |
+----+-----------------------------+----------------------------+
|  1 | json_string                 | {"a": "A", "b": "B"}       |
|  2 | json_object                 | {"a": "A", "b": "B"}       |
|  3 | json_object with list       | {"a": "[1,2,3]", "b": "B"} |
|  4 | json_object with json_array | {"a": [1, 2, 3], "b": "B"} |
|  5 | json_object with list       | {"test": "TEST"}           |
|  6 | json_string                 | {"a": "B", "b": "B"}       |
|  7 | json_string                 | {"a": "나다라", "b": "B"}  |
|  8 | json_string                 | {"a": "다라", "b": "B"}    |
|  9 | json_string                 | {"a": "라", "b": "B"}      |
| 10 | json_string                 | {"a": "마바사", "b": "B"}  |
| 11 | json_string                 | {"a": "고요요", "b": "B"}  |
| 12 | json_string                 | {"a": "1235", "b": "B"}    |
| 13 | json_string                 | {"a": "6542", "b": "B"}    |
+----+-----------------------------+----------------------------+
```

3) JSON 컬럼 전체 바꾸기
```sql
UPDATE jsontable SET jsoncol = '{"test":"TEST"}' WHERE id = 5;
```
**결과 예시:**
```
+----+-----------------------------+-----------------------------+
| id | name                        | jsoncol                     |
+----+-----------------------------+-----------------------------+
|  1 | json_string                 | {"a": "A", "b": "B"}        |
|  2 | json_object                 | {"a": "A", "b": "B"}        |
|  3 | json_object with list       | {"a": "[1,2,3]", "b": "B"}  |
|  4 | json_object with json_array | {"a": [1, 2, 3], "b": "B"}  |
|  5 | json_object with list       | {"test": "TEST"}            |
|  6 | json_string                 | {"a": "가나다라", "b": "B"} |
|  7 | json_string                 | {"a": "나다라", "b": "B"}   |
|  8 | json_string                 | {"a": "다라", "b": "B"}     |
|  9 | json_string                 | {"a": "라", "b": "B"}       |
| 10 | json_string                 | {"a": "마바사", "b": "B"}   |
| 11 | json_string                 | {"a": "고요요", "b": "B"}   |
| 12 | json_string                 | {"a": "1235", "b": "B"}     |
| 13 | json_string                 | {"a": "6542", "b": "B"}     |
+----+-----------------------------+-----------------------------+
```

4) JSON 컬럼에 키-값 추가
```sql
UPDATE jsontable SET jsoncol = JSON_INSERT(jsoncol, '$.test2', 'TEST2') WHERE id = 5;

UPDATE jsontable SET jsoncol = JSON_SET(jsoncol, '$.test2', 'TEST2') WHERE id = 5;
```
**결과 예시:**
```
+----+-----------------------------+-----------------------------+
| id | name                        | jsoncol                     |
+----+-----------------------------+-----------------------------+
|  1 | json_string                 | {"a": "A", "b": "B"}        |
|  2 | json_object                 | {"a": "A", "b": "B"}        |
|  3 | json_object with list       | {"a": "[1,2,3]", "b": "B"}  |
|  4 | json_object with json_array | {"a": [1, 2, 3], "b": "B"}  |
|  5 | json_object with list       | {"test": "TEST", "test2": "TEST2"} |
|  6 | json_string                 | {"a": "가나다라", "b": "B"} |
|  7 | json_string                 | {"a": "나다라", "b": "B"}   |
|  8 | json_string                 | {"a": "다라", "b": "B"}     |
|  9 | json_string                 | {"a": "라", "b": "B"}       |
| 10 | json_string                 | {"a": "마바사", "b": "B"}   |
| 11 | json_string                 | {"a": "고요요", "b": "B"}   |
| 12 | json_string                 | {"a": "1235", "b": "B"}     |
| 13 | json_string                 | {"a": "6542", "b": "B"}     |
+----+-----------------------------+-----------------------------+
```

5) JSON 컬럼에 키-값 제거
```sql
UPDATE jsontable SET jsoncol = JSON_REMOVE(jsoncol, '$.test2') WHERE id = 5;
```
**결과 예시:**
```
+----+-----------------------------+-----------------------------+
| id | name                        | jsoncol                     |
+----+-----------------------------+-----------------------------+
|  1 | json_string                 | {"a": "A", "b": "B"}        |
|  2 | json_object                 | {"a": "A", "b": "B"}        |
|  3 | json_object with list       | {"a": "[1,2,3]", "b": "B"}  |
|  4 | json_object with json_array | {"a": [1, 2, 3], "b": "B"}  |
|  5 | json_object with list       | {"test": "TEST"}            |
|  6 | json_string                 | {"a": "가나다라", "b": "B"} |
|  7 | json_string                 | {"a": "나다라", "b": "B"}   |
|  8 | json_string                 | {"a": "다라", "b": "B"}     |
|  9 | json_string                 | {"a": "라", "b": "B"}       |
| 10 | json_string                 | {"a": "마바사", "b": "B"}   |
| 11 | json_string                 | {"a": "고요요", "b": "B"}   |
| 12 | json_string                 | {"a": "1235", "b": "B"}     |
| 13 | json_string                 | {"a": "6542", "b": "B"}     |
+----+-----------------------------+-----------------------------+
```

6) JSON 키값 조회
```sql
SELECT id, name, JSON_KEYS(jsoncol) FROM jsontable;
```
**결과 예시:**
```
+----+-----------------------------+--------------------+
| id | name                        | JSON_KEYS(jsoncol) |
+----+-----------------------------+--------------------+
|  1 | json_string                 | ["a", "b"]         |
|  2 | json_object                 | ["a", "b"]         |
|  3 | json_object with list       | ["a", "b"]         |
|  4 | json_object with json_array | ["a", "b"]         |
|  5 | json_object with list       | ["test"]           |
|  6 | json_string                 | ["a", "b"]         |
|  7 | json_string                 | ["a", "b"]         |
|  8 | json_string                 | ["a", "b"]         |
|  9 | json_string                 | ["a", "b"]         |
| 10 | json_string                 | ["a", "b"]         |
| 11 | json_string                 | ["a", "b"]         |
| 12 | json_string                 | ["a", "b"]         |
| 13 | json_string                 | ["a", "b"]         |
+----+-----------------------------+--------------------+

```

7) 'ㄱ'으로 시작하는 단어 검색
```sql
SELECT * FROM jsontable WHERE jsoncol->'$.a' >= '가' AND jsoncol->'$.a' < '나';

SELECT * FROM jsontable WHERE jsoncol->'$.a' REGEXP '^"[가-깋]';

SELECT * FROM jsontable WHERE JSON_UNQUOTE(jsoncol->'$.a') REGEXP '^[가-깋]';
```
**결과 예시:**
```
+----+-------------+-----------------------------+
| id | name        | jsoncol                     |
+----+-------------+-----------------------------+
|  6 | json_string | {"a": "가나다라", "b": "B"} |
| 11 | json_string | {"a": "고요요", "b": "B"}   |
+----+-------------+-----------------------------+
```

8) 영어로 시작하는 단어 검색
```sql
SELECT * FROM jsontable WHERE jsoncol->'$.a' REGEXP '^"[A-Za-z]';

SELECT * FROM jsontable WHERE JSON_UNQUOTE(jsoncol->'$.a') REGEXP '^[A-Za-z]';
```
**결과 예시:**
```
+----+-------------+----------------------+
| id | name        | jsoncol              |
+----+-------------+----------------------+
|  1 | json_string | {"a": "A", "b": "B"} |
|  2 | json_object | {"a": "A", "b": "B"} |
+----+-------------+----------------------+
```

9) 숫자로 시작하는 단어 검색
```sql
SELECT * FROM jsontable WHERE jsoncol->'$.a' REGEXP '^"[0-9]';

SELECT * FROM jsontable WHERE JSON_UNQUOTE(jsoncol->'$.a') REGEXP '^[0-9]';
```
**결과 예시:**
```
+----+-------------+-------------------------+
| id | name        | jsoncol                 |
+----+-------------+-------------------------+
| 12 | json_string | {"a": "1235", "b": "B"} |
| 13 | json_string | {"a": "6542", "b": "B"} |
+----+-------------+-------------------------+
```

## 주의점
### 1) 시작 단어 필터링 시 주의점
- json column 안의 데이터는 "" 안에 씌워져 있기 때문에 첫 시작 단어를 " 로 넣고 필터링을 해야한다. 혹은 JSON_UNQUOTE 를 사용해야한다.

### 2) JSON_SET(), JSON_INSERT(), JSON_REPLACE() 차이점
- JSON_SET() : 기존 값을 바꾸고 존재하지 않는 값을 추가합니다. => 있으면 변경 + 없으면 추가
- JSON_INSERT() : 기존 값을 바꾸지 않고 값을 삽입합니다. => 없으면 추가
- JSON_REPLACE() : 기존 값만 바꿉니다 . => 있으면 변경

예시)
```sql
mysql> SET @j = '{ "a": 1, "b": [2, 3]}';
```
```sql
mysql> SELECT JSON_SET(@j, '$.a', 10, '$.c', '[true, false]');
+-------------------------------------------------+
| JSON_SET(@j, '$.a', 10, '$.c', '[true, false]') |
+-------------------------------------------------+
| {"a": 10, "b": [2, 3], "c": "[true, false]"}    |
+-------------------------------------------------+
```
```sql
mysql> SELECT JSON_INSERT(@j, '$.a', 10, '$.c', '[true, false]');
+----------------------------------------------------+
| JSON_INSERT(@j, '$.a', 10, '$.c', '[true, false]') |
+----------------------------------------------------+
| {"a": 1, "b": [2, 3], "c": "[true, false]"}        |
+----------------------------------------------------+
```
```sql
mysql> SELECT JSON_REPLACE(@j, '$.a', 10, '$.c', '[true, false]');
+-----------------------------------------------------+
| JSON_REPLACE(@j, '$.a', 10, '$.c', '[true, false]') |
+-----------------------------------------------------+
| {"a": 10, "b": [2, 3]}                              |
+-----------------------------------------------------+
```

#### 0. 참고 사이트
- https://givemethesocks.tistory.com/75
- https://www.joinc.co.kr/w/man/12/mysql/json

#### 0. 공식 문서
- https://dev.mysql.com/doc/refman/5.7/en/json-function-reference.html