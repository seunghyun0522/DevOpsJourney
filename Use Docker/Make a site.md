

<p align="center">
  <img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/6eb7b3f7-54b2-40da-809b-1240bc546249" width="30%"/>
</p>
Docker에는 다양한 컨테이너들이 존재한다. 

간단하게 사이트를 구축해보려고 한다. database는 mysql , 웹은 wordpress를 만들어보자!

root 계정에 접속되어 있고 docker가 설치되어 있다면 바로 시작할 수 있다.

EC2, 나만의 서버가 없다면, docker가 없다면?

⬇️ ⬇️ ⬇️

[AWS EC2 응용](https://cojjangsh.tistory.com/166)


### **Install**

기본적으로 설치할 때는 dockerhub에 들어가 검색해서 설치 방법을 보면 좋다

[도커허브 로그](https://hub.docker.com/)


dockerhub에 mysql을 검색한다.


  <img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/5a494993-4b60-43a8-8cbf-6951dfbf7027" width="60%"/>


install document를 확인해서 docker 내에서 설치하는 법을 숙지한다




  <img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/d472eebd-b1b9-4f5d-a6a6-7f375d30f9f9" width="60%"/>



-   **mysql install**
    -   \-d : 데몬 모드
    -   \--name : 이름설정
    -   \-e : 컨테이너에서 사용할 환경변수 
    -   mysql : 마지막에 설치할 컨테이너 이미지 mysql

```
docker run -d --name wordpressdb \
-e MYSQL_ROOT_PASSWORD=password \
-e MYSQL_DATABASE=wordpress mysql:5.7
```

[wordpress official | Docker hub](https://hub.docker.com/_/wordpress)



wordpress도 마찬가지로 똑같이 문서를 찾아서 사용하면 된다.

-   **wordpress install**
    -   \--link : 컨테이너끼리 연결한다 -> mysql과 연결
    -   \-p : 포트 포워딩

```
docker run -d -e WORDPRESS_DB_USER=root \
-e WORDPRESS_DB_PASSWORD=password \
--name wordpress \
--link wordpressdb:mysql \
-p 80:80 wordpress:5.6
```

설치가 끝났다면 images 를 확인한다.

```
docker images
```


  <img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/dd4811fd-ff22-4e80-939e-23df31aa5140" width="60%"/>

ubuntu,centos는 저번 포스팅에서 만들었고, mysql, wordpress가 돌아가는 것을 확인할 수 있다.

EC2아래에 보면 IP가 있다.  public ip를 접속해 보자. 접속은 ip:8080으로 접속하면 나온다.

  <img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/e67c6486-d9bc-42f0-8d49-1007cabccd3d" width="60%"/>




접속을 해보면...?? wordpress가 잘 나온다

  <img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/d5616ef5-7849-4851-9e20-30e1d14be78d" width="60%"/>



워드프레스를 설치하고 접속해 보자

  <img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/abf12b4a-7789-4ace-91e0-69652e3e5da6" width="60%"/>


잘 만들어지지만 관리자가 삭제하고 다시 만든다면 정보들이 사라진다(당연하지만)

docker image를 삭제하고 다시 실행해 보자

```
[root@ip-172-31-32-197 ~]# docker ps -a
CONTAINER ID   IMAGE           COMMAND                  CREATED         STATUS         PORTS                               NAMES
f7ebc512b98b   wordpress:5.6   "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp   wordpress
207125e5e3d0   mysql:5.7       "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes   3306/tcp, 33060/tcp                 wordpressdb
[root@ip-172-31-32-197 ~]# docker rm -f $(docker ps -a -q)
f7ebc512b98b
207125e5e3d0

docker run -d --name wordpressdb -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=wordpress mysql:5.7

docker run -d -e WORDPRESS_DB_USER=root -e WORDPRESS_DB_PASSWORD=password --name wordpress --link wordpressdb:mysql -p 80:80 wordpress:5.6
```

다시 접속해 보면 초기화 상태가 된다

  <img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/8164dfae-a0bd-42c9-9d1b-b643a11e3ebd" width="50%" height="400px"/>




그래서 사용하는 것이 docker 내에 **\-volume** 이라는 것이다.

### **Install with Volume**

Docker Volume은 Docker 컨테이너와 호스트 시스템 간에 데이터를 저장하고 공유하는 방법이다. 컨테이너는 일반적으로 임시이며, 내부 데이터는 컨테이너가 삭제될 때 같이 사라진다(위에 같이..) 그러나 Volume을 사용하면 데이터를 컨테이너 외부에 보관하여 컨테이너가 삭제되어도 데이터가 유지된다. 또한, 여러 컨테이너가 동일한 Volume을 공유하여 데이터를 공유할 수 있다.

DB를 volume으로 공유해 보자

-   Install MySQL with Volume
    -   \-v : 호스트와 컨테이너의 디렉토리를 연결해준다, 파일을 컨테이너에 저장하지 않고 호스트에 저장

```
docker run -d --name wordpressdb_v \
-e MYSQL_ROOT_PASSWORD=password \
-e MYSQL_DATABASE=wordpress \
-v /home/wordpress_db:/var/lib/mysql mysql:5.7
```

**\-v /home/wordpress\_db:/var/lib/mysql** 부분은 호스트 시스템의 **/home/wordpress\_db** 디렉토리를 컨테이너의 **/var/lib/mysql** 디렉토리에 마운트 하여 데이터 볼륨을 설정하는 부분이다. 이는 MySQL 데이터베이스 파일을 컨테이너 내부가 아닌 호스트 시스템의 해당 경로에 저장하겠다는 의미이다.

-   Install wordpress with MySQL -v
    -   똑같다.

```
docker run -d -e WORDPRESS_DB_USER=root \
-e WORDPRESS_DB_PASSWORD=password \
--name wordpress_v \
--link wordpressdb_v:mysql \
-p 80:80 wordpress:5.6
```

이렇게 하게 되면 서버가 삭제되고 이미지만 있다면 다시 실행 시 정보가 그대로 저장되어 있음을 확인할 수 있다.


  <img src="https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/1d55eb3f-56eb-493f-baeb-6f646fb3a723" width="60%"/>





일단 오늘은 자보자.. 나중에 해볼 포스팅 정리

일단 ubuntu, wordpress는 해봤으니 다른 웹 서버를 구축해 보는 것을 목표로 해보자!

\- 예를 들면... joomla, minecraft-server 등 여러 번 작동해서 익숙해지자

1.  도커 이미지 관리, 도커 허브로 이미지 관리
2.  Dockerfile을 만들어서 Container 구동
3.  구글 클라우드로 EC2와 같이 서버 생성
4.  쿠버네티스 사용, 여러 가지 서버 구축, 쿠버네티스 개념정리 -> 매우 오래 걸릴 예정
