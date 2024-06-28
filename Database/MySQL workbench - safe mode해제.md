# MySQL workbench - safe mode해제

### 1. Workbench의 설정 바꾸기

- MySQL 워크벤치 상단에서
  [ Edit - Preference - SQL Editor - 맨 아래의 Safe Updates 체크 해제]

- 체크 해제 후 MySQL 워크벤치 재시작

![image](../img/safe%20mode해제1.png)

![image](../img/safe%20mode해제2.png)

### 2. 직접 코드 입력하기

- SET SQL_SAFE_UPDATES = 0; 은 safe update mode를 해제하는 코드이고
- SET SQL_SAFE_UPDATES = 1; 은 다시 설정하는 코드이다.

```sql
SET SQL_SAFE_UPDATES = 0; 		-- 0 : sefe update mode 해제 , 1: safe update mode 설정
```
