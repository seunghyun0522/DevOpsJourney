![image](https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/5c83a6c6-816b-4019-abc5-cdb804d7a806)


### **AWS EC2**

EC2란 Amazon Elastic Compute Cloud의 줄임말로서 AWS에서 제공하는 클라우드 컴퓨딩이다. 독립된 컴퓨터를 임대하는 서비스, 즉 **컴퓨터를 대여해주는 서비스**이다. EC2는 사실 SaaS보다는 IaaS에 더 가까운 서비스다.

그럼에도 불구하고 SaaS를 구축하기 위해서는 앞으로 개발, 배포, 유지보수에 이르기까지 다양한 단계를 숙지해야한다. 또한 SaaS를 한다는 것은 서비스를 진행하는 것이기 때문에 서버 생성은 필수이다.

"_그렇기때문에 EC2에서 제공하는 가상 컴퓨터를 통해 다양한 클라우드 서비스를 공부해보자."_

---

### **AWS EC2 설치 및 실행**

-   접속 후 회원가입을 진행한다.(카드 등록 및 주소)

[https://aws.amazon.com/ko/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=\*all&awsf.Free%20Tier%20Categories=\*all](https://aws.amazon.com/ko/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all)



-   회원가입이 됐다면 location을 서울로 바꾸고 EC2를 선택한다

![image](https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/1212d6a5-39f7-4a3b-8d39-daaf252aab0b)


-   인스턴스 → 인스턴스 시작

![image](https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/0b34150d-dc67-476d-9037-4e4dc39cad99)


-   인스턴스의 이름을 설정(마음대로) 하고 프리티어 자원을 선택한다(AWS 프리티어에서 사용 가능한 VM 자원은 arm, x86이 있다)
    -   Ubunto Server로 설정

![image](https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/43790230-583a-4518-b865-d98794101337)


-   키페어를 생성 : 새 키 페어 생성 - 키를 페어링하는 것(퍼블릭, 프라이빗 키 생성)
    -   사용자 인증할 때 사용한다.

![image](https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/cfc27f99-9d8b-4846-8d3e-0c919cb50e21)


-   키 페어 생성
    -   RSA
    -   private key : `.ppk` ⇒ window
    -   키페어를 다운로드는 한번밖에 안된다. (다운로드를 받아 따로 저장해놓는다.)

![image](https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/2b76e645-bd30-4c59-8801-b4b5eff13650)


-   스토리지 구성
    -   최대 30G 이고 그 이상은 과금을 유발시킨다.
    -   루트 볼륨은 매번 업데이트가 되고 현재는 gp3로 설정한다.
    
  ![image](https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/b99ed1c2-a8e9-4489-b910-c010b5146963)

  -   그 후 인스턴스 ID를 클릭한다구성까지 다 했다면 인스턴스 생성을 누른다. 아래와 같이 인스턴스 상태가 실행 중으로 나온다. (약 5~10분 걸림)

![image](https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/e302d710-25fb-4be5-8470-abb3c0ac8075)


-   인스턴스 접속

![image](https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/431fff04-1db5-457c-89dc-b335f28eb135)


연결을 누른 후 다양한 방법이 있지만 aws에서 자체적으로 제공해주는 EC2 인스턴스 연결방식으로 접속을 한다. (터미널이 나온다)

![image](https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/9e03642a-8178-4253-956c-b6ca0e241da6)


터미널에 아래와 같이 입력한다

![image](https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/665fb5f2-ff8e-4cca-96df-781dd7741e24)


`whoami` : 사용자 이름을 확인 (그냥 확인하는거임)

`sudo su - root` : root 계정으로 접속한다.

```
ubuntu@ip-ip-port-name:~$ whoami
ubuntu
ubuntu@ip-ip-port-name:~$ ^C
ubuntu@ip-ip-port-name:~$ sudo su - root
root@ip-ip-port-name:~# whoami
root
```

_여기까지 했다면 이제 도커를 설치하고 사용해보자_

---

### **docker 설치 및 실행**

 도커의 개념을 정리했었다.

[https://cojjangsh.tistory.com/168](https://cojjangsh.tistory.com/168)


아래 글을 통해 간단하게 도커를 설치해보자!

[https://jinjinyang.tistory.com/46](https://jinjinyang.tistory.com/46)


#### docker 기본 실행

도커를 실행하기 전에 **docker run** 은 **Image** 로 **Container** 를 생성하는 명령어다.

사용하는 옵션들이 몇개 있어서 정리하고 가자. 많은 것들이 있지만 내가 사용했던것들만 정리해보자.

```
  docker run <옵션> <이미지> <명령> <매개 변수>
```

| \-i , --interactive | 표준 입력을 활성화, 컨테이너와 연결되어 있지 않아도 표준 입력을 유지 |
| --- | --- |
| \-t, --tty | TTY 모드 , Bash를 사용하려면 옵션 꼭 추가, 없으면 셀이 표시되지 않음 |
| \--name | 컨테이너 이름 설정 |
| \-d , --detach | 데몬 모드라고 부르는데 아직 잘 모르겠음 |
| \-p, --publish | 호스트와 컨테이너의 포트를 연결, <호스트 포트>:<컨테이너 포트> -p 80:80 |
| \--rm | 프로세스 종료시 컨테이너 자동 제거 |
| \-v, --volume | 데이터 볼륨을 설정, 마운팅한다고 한다. |

-   **도커 내에 내장 되어있는 centos 7 실행하기** 
    1.  docker run : docker를 가동하기 (접속)
    2.  centos:7 : image 7 version

```
root@ip-172-31-38-146:~# docker run -i -t centos:7
Unable to find image 'centos:7' locally
7: Pulling from library/centos
2d473b07cdd5: Pull complete Digest: sha256:be65f488b7764ad3638f236b7b515b3678369a5124c47b8d32916d6487418ea4
Status: Downloaded newer image for centos:7
[root@858886c8017c /]# docker ps
bash: docker: command not found
[root@858886c8017c /]# exit
```

-   **docker ubuntu 가져와서 실행하기**
    1.  pull을 했다는 것은 아직 실행한 것은 아니다. (단지 docker 내에 가져온 것이다)

```
#ubuntu version 20.04 로 접속

[root@ip-172-31-32-197 ~]# docker pull ubuntu:20.04
20.04: Pulling from library/ubuntu
d4c3c94e5e10: Pull complete 
Digest: sha256:874aca52f79ae5f8258faff03e10ce99ae836f6e7d2df6ecd3da5c1cad3a912b
Status: Downloaded newer image for ubuntu:20.04
docker.io/library/ubuntu:20.04

#images를 생성

[root@ip-172-31-32-197 ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       20.04     2abc4dfd8318   13 days ago   72.8MB
centos       7         eeb6ee3f44bd   2 years ago   204MB

#실행할 ubuntu 생성

[root@ip-172-31-32-197 ~]# docker create -i -t --name myubuntu ubuntu:20.04
f455eb49c2ff2b3d4396a3779e15d1fe83633d16fa6f08e61b758da1cde6e8a3

#docker 를 실행

[root@ip-172-31-32-197 ~]# docker start myubuntu
myubuntu

#실행시켰다면 접근하는 attach -> 이제 접근하는 것이다 (centos가 run 했던 상황)

[root@ip-172-31-32-197 ~]# docker attach myubuntu
root@f455eb49c2ff:/# ls
bin   dev  home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  etc  lib   lib64  media   opt  root  sbin  sys  usr

#종료가 아닌 잠시 서버를 체인지 하기 위한 키워드
Ctrl + P, Q
#변경됐다면 다시 접근
docker attach myubuntu
#docker 종료
exit
```

-   **docker 조회 및 컨테이너 이미지 삭제**

```
#docker 조회 
docker ps 
#docker 실행되는 모든 것을 조회
docker ps –a (docker의 모든 컨테이너 id와 이름은 동일, 줄여서 써도 됨) 
CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS                          PORTS     NAMES
f455eb49c2ff   ubuntu:20.04   "/bin/bash"   2 minutes ago   Exited (0) About a minute ago             myubuntu
a2db1d2c457f   centos:7       "/bin/bash"   8 minutes ago   Exited (127) 6 minutes ago                strange_hermann

#command에 "hello world!"를 추가한 ubuntu 실행
docker run -i -t ubuntu:20.04 echo hello world! 
#docker 이름 바꾸기
docker rename myubuntu renamed 
docker rename renamed myubuntu 
docker ps -a 

#docker 삭제 -> 실행중이면 삭제가 안된다
docker rm myubuntu 
#실행되는 docker 도 삭제
docker rm -f myubuntu 

#container 주소값 보여주기
docker ps -a -q 
#container 주소값을 통해 docker 컨테이너 멈추기
docker stop $(docker ps -a -q) 
#주소값을 통한 삭제
docker rm $(docker ps -a -q)
```

-   **ip확인, 접속된 컨테이너 전환하기, 서비스 실행**

```
#ip 확인
ifconfig 
#전환하기
Ctrl + P, Q 
#포트번호 80이고 ubuntu 실행
docker run -it --name mywebserver -p 80:80 ubuntu:14.04

#패키지 관리 툴인 apt-get을 이용하여 apache2를 설치한다. (apache2 설치와 함께 의존하는 프로그램도 함께 설치된다.)
#현재 사용 가능한 패키지와 그 버전에 대해서 리스트를 업데이트 해준다. 흔히 리눅스에서는 기본적으로 설치(install)되어 있는 리스트들에 대해서 다음과 같이 저장되어 있다. 
apt-get update
apt-get install apache2 -y

#service 실행
service apache2 start
```

_인스턴스 ip로 접속하면 접속이 되지 않는다._

ip를 로컬에서 구동하기 위해서는 `inbound`를 고쳐야한다.(필수)

기존 인스턴스로 이동해 보안 그룹을 클릭한다.

![image](https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/4f6e9772-106a-42c7-83c9-bd468f6b21be)


인바운드를 추가해야한다. -> 인바운드 규칙 편집을 누르고 규칙추가를 누른다.

![image](https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/3787dc5e-46b9-4185-9251-d3349b64782d)


규칙에는 유형 (HTTP) , 전체 허용(0.0.0.0/0)을 추가해준다.

![image](https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/ff2155e3-10ed-4af9-b5c9-384d0225a07a)


인바운드를 설정하게 된다면 아래와 같이 IP를 접속하면 ubuntu가 실행된다.

![image](https://github.com/seunghyun0522/DevOpsJourney/assets/75532258/b69eb560-3f7d-4dd3-94f4-7953d158f3f4)


자체적으로 컨테이너 이미지를 만들어 서버에 접속해서 ubuntu를 실행했다.

_다음 포스팅에서는 ubuntu를 실행했던 것처럼 wordpress를 실행시켜보고 volume을 추가해서 서버를 끄고 실행해도 저장되게 설정해보자._

_또한 도커허브에 있는 다양한 이미지들을 실습해서 정리해볼 예정이다._
