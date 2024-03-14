# MySQL 실행 쿼리 log 확인하는 방법

```sql
SET global general_log = 1;
SET global log_output = 'table'; -- 로그를 파일 대신 테이블에 저장하도록 설정

SELECT * FROM mysql.general_log ORDER BY event_time DESC;

SET global general_log = 0;

TRUNCATE table mysql.general_log;
```
