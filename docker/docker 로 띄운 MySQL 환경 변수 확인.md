## docker 로 띄운 MySQL 환경 변수 확인

```bash
docker inspect [컨테이너 이름 또는 ID] -f '{{ .Config.Env }}'
```
