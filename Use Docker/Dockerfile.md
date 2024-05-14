<p align="center">
  <img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/6eb7b3f7-54b2-40da-809b-1240bc546249" width="30%"/>
</p>

## **Docker build , run**

`docker build`와 `docker run`은 Docker에서 사용하는 두 가지 주요 명령어로, 각각 다른 목적을 가지고 있다. 이 개념을 이해할 때 수행하는 작업의 단계와 목적을 생각하면 이해하기 좋다.

### Docker build

`Dockerfile`에 정의된 지침에 따라 Docker 이미지를 생성한다(이 과정에서 커스터마이징이 이루어진다). 주요 작업은 소프트웨어 설치, 환경 변수 설정, 파일 복사 등 Dockerfile에 명시된 모든 지침을 실행해서 이미지를 만든다.  
결국 재사용 가능한 Docker 이미지(컨테이너를 실행하기 위한 "템플릿") 역할을 한다.

### Docker run

이미 생성된 Docker 이미지를 바탕으로 새로운 컨테이너를 생성하고 실행한다. 이미지로부터 컨테이너를 생성하고, 컨테이너 내에서 사용자가 지정한 명령을 실행한다. 사용자가 지정한 작업을 수행한다.

`docker run`과 `docker build`는 결국 이미지 생성(build)과 이미지를 바탕으로 한 컨테이너 실행(run)이라는 두 단계의 다른 목적과 작업에 있다. `build`는 이미지를 만드는 단계, `run`은 이미지를 사용하여 실제 작업을 수행할 컨테이너를 실행하는 것이다.



</br>

## **Dockerfile**

`Dockerfile`는 도커 이미지를 생성하기 위한 스크립트(설정파일)이다.

`Dockerfile 작성 -> build -> run` 의 순서로 docker를 실행한다.

### Dockerfile 작성

테스트를 하기 위해서 `test.html`을 생성해준다.

`test >> test.html` 을 실행

`Dockerfile`을 작성할 때는 실제 파일 이름 `Dockerfile`로 만들어야 한다.

```
vi Dockerfile
```

`FROM` : 기본 이미지 설정  
`MAINTAINER` : 이미지 생성자 지정 (잘 안사용하는 듯?)  
`LABEL` : 이미지에 메타 데이터를 추가(키 - 값)  
`RUN` : 새로운 이미지를 생성  
`ADD` : 파일 추가  
`WORKDIR` : 작업 디렉터리를 지정  
`EXPOSE` : Docker에 열어줄 포트 설정  
`CMD` : 컨테이러르 생성할 때만 실행된다.(docker run)

```
# 기본 이미지 설정 -> ubuntu:18.04를 기본 이미지로 사용한다.
FROM ubuntu:18.04

# 이미지 생성자 지정 -> 이미지의 생성자 또는 유지 관리자를 {user_name}으로 지정한다.
MAINTAINER seunghyun

# 레이블 추가 -> 이미지에 메타데이터를 추가한다.  (키-값) 쌍으로 레이블을 추가한다.
LABEL "course"="Seunghyun Study"

# 패키지 업데이트 및 설치 -> 패키지 목록을 업데이트하고 Apache2 웹 서버를 설치한다. -y옵션은 설치 중 질문에 자동으로 'yes'로 응답하도록 해준다.
RUN apt-get update
RUN apt-get install apache2 -y

# 파일 추가 -> 호스트 시스템의 test.html 파일을 이미지의 /var/www/html 디렉토리로 추가한다.
ADD test.html /var/www/html 

# 작업 디렉토리 변경 -> 이미지 내에서의 작업 디렉토리를 변경한다. 이후의 명령어들은 이 디렉토리에서 실행된다.
WORKDIR /var/www/html

# 파일 수정 -> /bin/bash 쉘을 이용해서 echo hello >> test2.html 명령어를 실행한다.
RUN ["/bin/bash", "-c", "echo hello >> test2.html"]

# 포트 노출 -> 컨테이너의 80번 포트를 노출한다.
EXPOSE 80 

#컨테이너 실행 명령어 -> 컨테이너가 시작될 때 실행될 기본 명령어를 지정한다. Apache 웹 서버를 포그라운드 모드로 ㅣㄹ행한다.
CMD apachectl -DFOREGROUND
```

### Image build & run

`docker build -t [이미지 이름:이미지 버전] [Dockerfile의 경로]` 를 통해 build를 해준다. Dockerfile 이름으로 생성했다면 끝에 `./`으로 해준다.

```
docker build -t mybuild:0.0 ./

###### loading.....####

kseounghyn12@cloudshell:~$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED              SIZE
mybuild      0.0       8e6212fd88bf   About a minute ago   205MB

#실행!
docker run -d -p 80:80 --name myserver mybuild:0.0
```
</br>

## Dockerfile을 통해 jupyter Server 만들기

Docker 내에 저장되어 있는 container를 가져와 구동시키고 jupyter notebook을 실행시키기 위해 pytho 패키지 관리자를 설치한다.

```
# port 8888 , ubuntu
docker run -it --name jupyter -p 8888:8888 ubuntu:20.04

# update
apt-get update
# python 패키지 install
apt-get install python3 python3-pip -y

# jupyter를 설치
pip3 install jupyter

#jupyter note file을 저장하기위해 생성
mkdir /notebooks


# 모든 ip 가 접속, port번호 8888, allow-root => jupyter는 기본적으로 루트 사용자로 실행되는 것을 허용하지 않음
# notebooks 에 저장
# jupyter notebook 서버의 루트 디렉토리를 notebooks로 설정
# & 백그라운드에 실행될 수 있게
jupyter notebook --ip=0.0.0.0 --port=8888 --allow-root --notebook-dir=/notebooks &
```

여러 번 입력하는 것도 좋지만 문서화를 시켜서 나중에 버전을 바꾸거나 build 하기 용이하게 바꿔준다.

```
# 실행 중인 컨테이너 삭제
docker rm -f $(docker ps -a -q)

# 위에 ubunutu와 매우 비슷하다
vi Dockerfile
```

Dockerfile

```
FROM ubuntu:20.04
# image creator
RUN apt-get update
RUN apt-get install python3 python3-pip -y
RUN pip3 install jupyter
RUN mkdir /notebooks
CMD jupyter notebook --ip=0.0.0.0 --port=8888 --allow-root --notebook-dir=/notebooks
```

실행

```
docker run -d -p 8888:8888 --name myjupyter -v ~/work:/notebooks seunghyun/myjupyter:1.0
```
