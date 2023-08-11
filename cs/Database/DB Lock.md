# DB Lock 종류

## 1. Lock이란?

데이터의 일관성을 보장하기 위한 방법입니다. 오라클과 같이 고가의 DBMS를 사용하는 이유가 데이터의 무결성과 일관성을 유지하는 능력이 뛰어나기 때문입니다.

## 2. DB 락의 종류

데이터베이스에서 사용되는 주요한 락의 종류는 다음과 같습니다:

### 2.1 Shared Lock(공유 Lock 또는 Read Lock)

- 보통 데이터를 읽을 때 사용합니다.
- 원하는 데이터에 lock을 걸었지만 다른 세션에서 읽을 수 있습니다.
- 공유Lock을 설정한 경우 추가로 공유Lock을 설정할 수 있지만, 배타적 Lock은 설정할 수 없습니다.
  
### 2.2 Exclusive Lock(배타적 Lock 또는 Write lock)

- 보통 데이터를 변경할 때 사용합니다.
- 배타적 Lock이 설정된 데이터에는 다른 공유Lock, 배타적Lock을 설정할 수 없습니다.

## 3. Blocking

블로킹은 Lock들의 경합(Race condition이라고도 합니다)이 발생하여 특정 세션이 작업을 진행하지 못하고 멈춰 선 상태를 의미합니다. 이를 해결하는 방법은 Transaction commit 또는 rollback 뿐입니다.

## 4. Dead Lock

Deadlock은 트랜젝션간의 교착상태를 의미합니다. 두개의 트랜젝션간에 각각의 트랜젝션이 가지고 있는 리소스의 Lock을 획득하려고 할 때 발생합니다.

## 5. Lock Level & Escalation

### 5.1 Row level

- 변경하려는 row에만 lock을 설정하는 것을 의미합니다.

### 5.2 Page level

- 변경하려는 row가 담긴 데이터 page (또는 인덱스 페이지)에 lock을 설정합니다. 같은 페이지에 속한 row들은 변경작업과 무관하게 모두 lock에 의해 잠깁니다.

### 5.3 Table level

- 테이블과 인덱스에 모두 잠금을 설정합니다. Select table, Alter table, Vacuum, Refresh, Index, Drop, Truncate 등의 작업에서 해당레벨의 락이 설정됩니다.

### 5.4 Database level

- 데이터베이스를 복구하거나 스키마를 변경할 때 발생합니다.

### 5.5 Escalation

Lock리소스가 임계치를 넘으면 Lock 레벨이 확장되는 것을 의미합니다. Lock 레벨이 낮을 수록 동시성이 좋아지지만, 관리해야할 Lock이 많아지기 때문에 메모리 효율성은 떨어집니다. 반대로 Lock레벨이 높을 수록 관리리소스는 낮지만, 동시성은 떨어집니다.
