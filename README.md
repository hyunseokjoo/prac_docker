# 목차
- [docker workFlow 이해하기](#docker-workFlow-이해하기)
- [기본 명령어 정리](#기본-명령어-정리)
- [docker file 작성법](#DockerFile-작성법)
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


# DockerFile 작성법
DockerFile에 대해 깊게 이해하고 싶으면 아래의 링크를 타고 가자 
[Dockerfile reference](https://docs.docker.com/engine/reference/builder/)

## Dockerfile이란

Docker Image를 만드는 스크립트이다. 미리 짜여져 있는 Image를 사용하는 것이 아니라 Custom하여 Image를 만드는 것이라고 보면 된다.

## 레이어 시스템

도커파일을 만들 때 레이어 시스템이라는 시스템을 사용하는데 아래 작성한 내용을 명령어 당 한줄 씩 한 레이어라고 칭한다. 맨처음 이미지 빌드하는 경우 모든 레이어가 다 호출 되지만, 그 다음 부터는 변경된 것만 파악하여 빌드를 하게 되는데 이때 cache를 많이 잡아 먹고 속도를 개선하기 위해서는 많이 변경되는 레이어를 아래에 두어야 한다. 변경된 것을 파악한 시점 부터 모두 재빌드 하기 때문 예를 들어 1~5레이어로 구성된 dockerfile이 있다고 하고, 3번째 레이어에서 변경된 것이 파악이 되면 1~2번째 까지는 다시 빌드하지 않고 Cache에 저장되어있는 상태를 그대로 사용하고 3~끝까지는 다시 image를 만든다.

## DockerFile 명령어

| FROM | 생성할이미지의 베이스가되는 이미지 |
| --- | --- |
| LABEL  | 이미지에 메타데이터를추가 |
| WORKDIR | 생성된 컨테이너 안에서 명령어를 실행할 디렉터리를 나타냄 |
| COPY | 호스트에서 이미지에 파일 복사하여 추가 |
| ADD | 호스트안에 있는 파일/디렉토리 추가, COPY와 다른점음 압축파일은 해제하고 추가하고, 또한 wget이라는 명령어를 이용하여 추가도 가능 |
| EXPOSE | 호스트와 연결할 포트 번호를 설정(보통 명령어run 에서 -p로 대체) |
| ENV | 환경변수 설정 |
| RUN  | 이미지를만들기 위해 컨테이너 내부에서 돌아갈 명령어 이미지가 완전 생성 되지 전에 실행 되는 명령어이다 |
| CMD | 컨테이너가 실행 되었을 때 바로실행되는 명령어 설정 |
| ENTRYPOINT  | 컨테이너가 시작되었을 때 스크립트실행 |
| VOLUME | 호스트 주소와 마운트(DB는 파일을 가지고 있어야하기 때문에 마운트 필수) |

DockerFile 작성 예시

```docker
# 이미 구현되어있는 이미지 명시
FROM {baseImage명}:{version}
FROM ubuntu:18.04

# baseImage안에서 사용할 명령어를 Run으로 작성
# -y 옵션은 ubuntu에서 사용하는 yes 명령어임 
# RUN 을 COPY한다음 실행해야하는 경우이면 COPY한다음 RUN을 또 실행해 주면 된다.
RUN {해당 컨테이너에 맞는 명령어} && {&&사용하면 두개이상 가능}
RUN apt-get updates && apt-get instll -y --force-yes apache2

# 환경 변수 설정
# FOO라는 변수에 '/bar'라는 값을 넣은 것임
ENV {환경 변수명}={변수}
ENV FOO=/bar
# WORKDIR ${FOO} <- 이렇게 사용가능
# WORKDIR /bar 라는 의미

# 컨테이너 안에서 어떤 경로에서 작업할 것인지 표시
WORKDIR {컨테이너 안에서 사용할 경로}
WORKDIR /home/app

# 현태 디렉토리 주소의 파일들을 다 static_assets에 넣겠다는 의미
# 호스트 경로를 아래와 같이 적으면 c드라이브 users 아래 app폴더의 내용을 모두 복사하겠다는 의미
# 편리 하게사용 하기 위해서 터미널에서 미리 이동한 다음 실행하면 아래와 같이 모든 경로를 적지 않아도 된다.
# 컨테이너 안에 경로를 ./로 하겠다고 표시하면 WORKDIR에 넣겠다는 의미
COPY {호스트에서 가져올 파일이나 디렉토리 경로} {컨테이너 안에 경로}
COPY C:\Users\app ./

# 해당 파일을 가져가는 것은 똑같은데 
# tar gz 같은 파일은 모두 압축해제하여 추가한다.
# 로컬 파일 대신 wget명령어를 지원하여 추가 할 수 있다.
# 컨테이너 '/work/dir'주소에 현재 호스트 pwd안에 hom으로 시작하는 파일 모두 추가
ADD hom* /work/dir/

# 도커 run 시 아무런 명령어를 주지 default 명령어를 적어준다
# CMD ['executable', 'param1', 'param2']
# CMD ['param1', 'param2']
# python으로 manage.py를 실행한다면 아래와 같이
# python manage.py 와 똑같은 명령어이다.
CMD ['python', 'manage.py']
```

## CMD와 ENTRYPOINT이해하기

CMD명령어는 run시에 default로 실행되는 명령어를 적용한다.

```docker
FROM ubuntu
CMD echo "This is a test.
```

```bash
$ docker run -it --rm <image-name>
This is a test
$
```

```bash
$ docker run -it --rm <image-name> echo "Hello"
Hello
$
```

ENTRYPOINT는 run 시에 cmd의 Default값을 합쳐서 명령어를 적용한다.

```docker
# CMD랑 똑같은 역할을 하지만, CMD가 설정이 되어있다면, CMD를 가져와 사용한다.
ENTRYPOINT
FROM ubuntu
ENTRYPOINT ["/bin/echo", "Hello"]
CMD ["world"]
```

```bash
$ docker run -it --rm <image-name>
Hello world
$ docker run -it --rm <image-name> ME
Hello ME
$
```

```bash
FROM ubuntu
ENTRYPOINT [ "echo", "$HOME" ]

$ docker run -it --rm <image-name>
$HOME
$
```

## DockerFile 빌드하기

```bash
# dockerfile 빌드하기 
docker build -t {이미지명} {작성한 도커파일 경로}
docker build -t frontend   .
# 해당 디렉토리에 이동한후 Dockerfile이라고 생성하고 build를 하면 dockerfile명을 적지 않고 
# .으로 적어도 무방하다.

# dockerfile 로 빌드한 이미지 실행 
docker run --name {실행image명} -v $(pwd):{workdir} -p {입력포트}:{컨테이너내부포트} {컨테이너명}
docker run --name front         -v $(pwd):/home/app -p 8080:8080  front-container
```
