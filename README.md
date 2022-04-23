#목차
- 기본 명령어 정리
- docker file 작성법
- docker compose 

# 기본 명령어 정리

- 컨테이너 내려받기

```
nginx컨테이너를 받는 명령어는 
pull옵션을 사용한다
TAG를 사용하여 버전 적어 
버전별로 받을 수 있음 

# docker pull NAME[:TAG]
$ docker pull nginx:latest
```

- 컨테이너 실행

```
docker run명령어를 사용하여 
다운로드 받은 image를 실행가능, 
image가 없다면 다운받고 실행한다.

- 포그라운드로 실행
# docker run [OPTION] IMAGE[:TAG] [COMMAND]
$ docker run -it ubuntu:16.04 bash

- 백그라운드(데몬으로 실행)
$ docker run -d -p 80:80 nginx
```

- 컨테이너 이름 할당

```
-name옵션을 주면 image를 받을 때 
그 이름을 지정해 줄 수 있다. 
name옵션을 주지 않으면 임의로 
docker에서 이름을 지정해 주니 
간편하게 조작하기 위해 name을 꼭 주자

$ docker run -i -t --name my_ubuntu ubuntu:16.04 /bin/bash
```

- 컨테이너 포트 포워딩

```
해당 image를 실행 할 때 포워딩을 
사용하여 포트를 맞춰 줄 수 있다.
여러개를 사용하고 싶다면, 
-p 포트:포트를 여러개 사용한다.

$ docker run -d --name my_nginx -p 80:80 -p 3306:3306 nginx:latest
```

- 컨테이너 종료

```
# 컨테이너 종료
$ exit

# 컨테이너 나오기 
$ Ctrl + P,Q
```

- 컨테이너 목록 확인

```
$ docker ps

-a 옵션은 만들어져 있는 
모든 컨테이너 내용을 확인 가능하다
종료 된 것은 언제 종료됬는지도
확인 가능하다.
$ docker ps -a
```

- 컨테이너 조작 명령어

```
컨테이너 시작
$ docker start my_ubuntu

컨테이너 내부로 들어가기
$ docker attach my_ubuntu

컨테이너 정지
$ docker stop my_ubuntu

컨테이너 삭제 
$ docker rm my_ubuntu 

컨테이너 삭제 한번에
-f는 force라는 뜻임
$ docker rm -f my_ubuntu 

컨테이너 종료 시 자동 삭제 옵셔 
$ docker run -it -rm my_ubuntu 
```

- 이미지 관련 명령어

```
도커 이미지 목록
$ docker iamges

도커 이미지 삭제
$ docker rmi ubuntu:16.04
```
