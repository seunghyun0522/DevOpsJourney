

<p align="center">
  <img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/6eb7b3f7-54b2-40da-809b-1240bc546249" width="30%"/>
</p>

## 도커 서버의 종류

도커를 공부하면서 OS, Web, Database 등 다양한 콘텐츠를 가져올 수 있다 보니 어떤 것이 OS인지 Web인지 헷갈리기도 했고 사용법이 다 다르다는 것을 공부하면서 알게 됐다.

도커를 사용할 때 우리가 `서버`라고 할 때는, 도커 컨테이너 내에서 실행되는 다양한 종류의 서비스나 애플리케이션(웹서버, 데이터베이스 서버 등)을 가리킨다. 컨테이너는 이러한 서비스를 격리되고 일관된 환경에서 실행하기 위한 기술적 수단이다.

#### 컨테이너(Container)


<p align="center">
<img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/669dddbf-7f46-41a6-92ad-e51263a5003c" width="60%"/>
</p>




도커는 컨테이너화된 애플리케이션을 실행하기 위한 플랫폼이다. 컨테이너는 애플리케이션과 그 애플리케이션을 실행하는 데 필요한 모든 것(코드, 런타임, 시스템 도구, 시스템 라이브러리 등)을 패키지화하여, 애플리케이션이 어느 환경에서든 일관되게 실행될 수 있도록 해준다. 따라서, 컨테이너 자체를 '서버의 한 종류'라기보다는, 다양한 유형의 서버(웹서버, 데이터베이스 서버 등)를 실행하는 환경이라고 이해하는 것이 더 정확하다.

#### 웹서버(Web Server)

<p align="center">
<img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/ee42b81e-6189-48d1-a3f7-4f0d5534bbffc" width="60%"/>
</p>


클라이언트(예: 웹 브라우저)의 요청을 받아 HTML 문서나 오브젝트(예: 이미지, 파일 등)를 반환하는 서버다. `Apache`, `Nginx`와 같은 웹서버 소프트웨어는 도커 컨테이너 내에서 실행될 수 있으며, 이를 통해 웹 애플리케이션을 호스팅 하는 데 사용된다.

#### 데이터베이스 서버(Database Server)

<p align="center">
<img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/116c11ef-1aa9-41e3-b421-b7a7ce361554c" width="60%"/>
</p>



데이터를 구조화하여 저장하고, 관리하며, 검색할 수 있도록 설계된 서버다. MySQL, PostgreSQL, MongoDB와 같은 데이터베이스 관리 시스템(DBMS)은 데이터를 안정적으로 저장하고, 필요에 따라 빠르게 접근할 수 있도록 해준다. 이러한 DBMS 또한 도커 컨테이너로 실행될 수 있다.

---

## 도커 이미지 관리

#### **터미널에 저장**

-   docker에 로그인을 한다

```
{google_cloud_id}@cloudshell:~$ docker login

#Log in with your Docker ID or email address to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com/ to create one.
#You can log in with your password or a Personal Access Token (PAT). Using a limited-scope PAT grants better security and is required for organizations using SSO. Learn more at https://docs.docker.com/go/access-tokens/

Username: {user_name}
Password: 
#WARNING! Your password will be stored unencrypted in /home/{user_name}/.docker/config.json.
#Configure a credential helper to remove this warning. See
#https://docs.docker.com/engine/reference/commandline/login/#credentials-store

#Login Succeeded
```

-   이미지 구동 (나는 ubuntu 컨테이너 서버를 띄울 것이다)

```
docker run -it --name docker_study ubuntu 
```

설치와 접속이 완료되면 ubunutu 에 접속되어 있음을 확인할 수 있다. `root`

```
root@73ef47157eb1
```

이곳에 `first` 라는 폴더 내에 `test_first` 라는 message를 넣는다.

접속된 ubuntu 서버를 나가기 위해 `Ctrl P+Q` 를 통해 나간다.

기존 이미지에서 commit을 해주고 이미지를 확인해 본다.

```
docker commit -a "seunghyun0522" -m "my first commit" docker_study commit_test:first
docker images
```

```
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
commit_test   first     7c264d47cfb6   3 seconds ago   76.2MB
ubuntu        latest    bf3dc08bfed0   13 days ago     76.2MB
```

#### **도커허브에 저장**

도커허브에서 회원가입

[도커 허브](https://hub.docker.com/)

`Create repository` 생성

<p align="center">
  <img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/b74ba925-f016-48a8-a295-368b32638999" width="60%"/>
</p>




repo 생성

<p align="center">
  <img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/35f58308-d009-4c2d-9fbb-00ba8b54d0ce" width="60%"/>
</p>





Shell에서 도커허브로 push

```
# ubuntu 서버를 구동
docker run -it --name commit_container1 ubuntu:14.04
# ubuntu 파일 생성
 echo my first push >> test
# commit 내장에 저장
docker commit commit_container1 test_ubunutu:0.0
# tag 해서 도커허브에 올릴 이미지 생성(복사)
docker tag test_ubunutu:0.0 seunghyun0522/test_ubunutu:0.0
# 도커허브 login
docker login
# 도커허브 push
docker push seunghyun0522/test_ubunutu:0.0
```

도커허브에 확인하면 push 된 것을 확인할 수 있다.

<p align="center">
  <img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/026ae1ab-14ff-4a36-92a4-fa3fa25bfcfd" width="60%"/>
</p>



push 된 것을 확인 후 기존의 images를 삭제 후 pull로 가져오면 똑같이 사용할 수 있다.(다른 유저도 사용 가능)

```
docker pull seunghyun0522/test_ubunutu:0.0
```
