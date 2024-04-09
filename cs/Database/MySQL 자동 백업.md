# MySQL 자동 백업

### Shell script 작성

1. 백업용 스크립트 작성

```
$ vim backup.sh
```

```bash
#!/bin/bash

# 데이터베이스 설정
DB_USER="user_id" # db username
DB_PASSWORD="user_pw" # db password
DB_NAME="saleskitdb" # schema name
BACKUP_PATH="/dumps" # backup path

# 날짜 포맷 설정 (예: backup-20240409.sql)
DATE=$(date +%Y%m%d)
BACKUP_FILE="$BACKUP_PATH/backup-$DATE.sql"

# MySQL 백업 명령 실행
mysqldump -u $DB_USER -p$DB_PASSWORD --databases $DB_NAME > $BACKUP_FILE

# 백업 파일 압축
gzip $BACKUP_FILE
```

2. 일정 기간이 지나면 삭제되게 설정

```
$ vim clean.sh
```

```bash
#!/bin/bash

BACKUP_PATH="/appdat/saleskit/dumps"
# 7일 보다 오래된 파일 삭제
find $BACKUP_PATH -name "*.gz" -type f -mtime +7 -exec rm {} \;
```

### crontab 설정

```
$ crontab -e
```

```vim
...

0 0 * * * /backup.sh > /logs/backup.log 2>&1 || true
0 0 * * * /clean.sh > /logs/cleanup.log 2>&1 || true

...
```
