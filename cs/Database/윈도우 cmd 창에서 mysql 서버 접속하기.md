# 윈도우 cmd 창에서 mysql 서버 접속하기(참고 : https://byul91oh.tistory.com/259)

1. MySQL 다운로드 (https://dev.mysql.com/downloads/mysql/)

2. Path 설정
- 키보드의 [윈도우 버튼 + PauseBreak 버튼] 혹은 [시작 - 설정(톱니바퀴모양) - 시스템 - 정보] 메뉴로 들어가 화면 우측 또는 하단의 [고급 시스템 설정] 클릭
- 시스템 속성 창에서 우측 하단의 [환경 변수] 버튼을 클릭
- Path 편집
- Mysql 경로 추가 (ex : C:\Program Files\MySQL\MySQL Server 5.7\bin)
- [확인] 버튼 클릭

3. MySQL 접속
- cmd창 실행
- mysql -u root(유저네임) -p 입력
- password 입력