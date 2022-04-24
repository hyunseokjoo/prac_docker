# 목차
- [docker workFlow 이해하기](#docker-workFlow-이해하기)
- [기본 명령어 정리](#기본-명령어-정리)
- docker file 작성법
- docker compose 

# docker workFlow 이해하기
Docker에서는 Container단위로 가상화를 app을 실행하게 되는데 Container의 구성요소를 잘 알아야 한다. Container의 이해는 아래 링크로 확인

[20분 만에 전공자처럼 도커, 가상화 이해하기!](https://www.youtube.com/watch?v=zh0OMXg2Kog&list=WL&index=21&t=431s)

### Container 3대 구성요소

- Container
- Image
- Docker File
![docker_workflow](https://user-images.githubusercontent.com/49854618/164966561-7bfea1aa-6faa-4f74-97ad-7900509b1b60.png)
- Container는 하나 이상의 image로 구성되어 있다.
- Image는 Docker Hun에서 가져와 사용도 가능하고, Docker File로 Custom하여 생성, 관리, 사용이 가능하다
- Container는 독립적인 공간으로 구성되기 때문에 local에서 Container로 접근이 가능하지만, Container끼리는 접근이 불가능하다.

### Docker Flow

- Local 에서 작업 하여 Image생성
- 해당 Image Docker Hub에 Push하여 올림
- Server 환경에서 Pull 받아 Image를 사용한다.
![docker workflow2](https://user-images.githubusercontent.com/49854618/164966558-ef33d506-8270-4ac1-9545-18e1b6a13728.png)
- Docker Hub - Git Hub와 동일한 방식인데 Repository같은 작업 공간들을 Hub에 올려 놓고 공유하거나 관리가 가능하게 만들어 놓은 공간이다. Github 처럼 web상으로 관리된다. 아래 link를 타고 들어가 확인해보자. [docker hub](https://hub.docker.com/)
- Container Registry는 Git hub의 repository라고 생각하면 된다.


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
