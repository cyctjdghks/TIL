### Docker 명령어

1. **도커 컨테이너 관련 명령어**:
    
    - `docker run [옵션] [이미지] [명령]`
        
        - `-d`: 백그라운드 실행. 예) `docker run -d nginx`
        - `--name`: 컨테이너 이름 지정. 예) `docker run --name my_container nginx`
        - `-p`: 포트 매핑. 예) `docker run -p 80:80 nginx`
        - `-v`: 볼륨 마운트. 예) `docker run -v /host/dir:/container/dir nginx`
        - `-e`: 환경 변수 설정. 예) `docker run -e MY_VAR=value nginx`
        - `--rm`: 종료 시 컨테이너 자동 삭제. 예) `docker run --rm nginx`
        - `--link`: 다른 컨테이너에 연결(네트워킹). 예) `docker --link db`
    - `docker ps`
        
        - `-a`: 모든 컨테이너 보기. 예) `docker ps -a`
    - `docker stop/start/restart [컨테이너ID/이름]`
        
    - `docker rm`
        
        - `-f`: 강제로 컨테이너 삭제. 예) `docker rm -f my_container`
    - `docker logs [컨테이너ID/이름]`
        
        - `-f`: 실시간 로그 표시. 예) `docker logs -f my_container`
    - `docker exec`
        
        - `-it`: 대화형 모드. 예) `docker exec -it my_container bash`
    - `docker attach [컨테이너ID/이름]`
        
2. **도커 이미지 관련 명령어**:
    
    - `docker images`
        
        - `-a`: 중간 이미지까지 모두 표시. 예) `docker images -a`
    - `docker rmi [이미지ID/이름]`
        
        - `-f`: 강제로 이미지 삭제. 예) `docker rmi -f nginx`
    - `docker pull [이미지이름]`
        
        - 예) `docker pull nginx`
    - `docker push [이미지이름]`
        
        - 예) `docker push my_image`
    - `docker build`
        
        - `-t`: 이미지 태그 지정. 예) `docker build -t my_image:1.0 .`
3. **도커 네트워크 관련 명령어**:
    
    - `docker network ls`
        
    - `docker network create [옵션] [네트워크 이름]`
        
        - `--driver`: 네트워크 드라이버 지정 (예: bridge, overlay). 예) `docker network create --driver bridge my_network`
    - `docker network rm [네트워크 이름]`
        
    - `docker network connect/disconnect [네트워크 이름] [컨테이너ID/이름]`
        
4. **도커 볼륨 및 저장 관련 명령어**:
    
    - `docker volume ls`
        
    - `docker volume create [옵션] [볼륨 이름]`
        
        - `--name`: 볼륨 이름 지정. 예) `docker volume create --name my_volume`
    - `docker volume rm [볼륨 이름]`
        
5. **기타 도커 명령어**:
    
    - `docker info`
        
    - `docker version`
        
    - `docker-compose` (도커 컴포즈는 여러 컨테이너를 동시에 관리하는 도구로, 별도의 설치가 필요합니다.)