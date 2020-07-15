



1. Git이란? 

https://git-scm.com/book/ko/v2 (공식 Site 한글 매뉴얼))

1. it은 소프트웨어를 개발하는 기업의 핵심 자산인 소스코드를 효과적으로 관리할 수 있게 해주는 무료, 공개소프트웨어
2. SVN보다 여러 장점이 있어 SVN을 쓰던 개발 조직들은 하나둘씩 Git으로 갈아타고 있다.



1.2 SVN과 Git의 차이점

 - Git이 SVN과 다른 점은 분산형 관리 시스템이라는 것이다.

 - SVN : 중앙 서버에 소스코드와 히스토리를 저장하는 과 달리

   Git :  소스코드를 여러 개발 PC와 저장소에 분산해서 저장

   그렇기 때문에 중앙 서버에 장애가 발생해도 로컬 저장소에 커밋을 할 수 있으며, 로컬 저장소들을 이용하여 중앙 저장소의 복원도 가능하다.

 - 사본을 로컬에서 관리하기 때문에 GIT이 SVN에 비해 훨씬 빠르다.(SVN은 변경 로그 하나 보는 것도 인터넷을 경유해야 한다.)
 
- ![텍스트](https://git-scm.com/book/en/v2/images/lifecycle.png)]



git Docker image 만들기

root@tmax-22cf9064:~/temp# cat Dockerfile
FROM ubuntu:latest 
RUN apt-get update
RUN apt-get install -y git

root@tmax-22cf9064:~/temp# docker build -t git-ubuntu:v1 .
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM ubuntu:latest
 ---> adafef2e596e
 
 root@tmax-22cf9064:~/temp# docker images
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
git-ubuntu                    v1                  87ad665933f4        59 seconds ago      198MB

root@tmax-22cf9064:~/temp# docker run -idt git-ubuntu:v1
0ced7a4937e3654059bc9e8aae243a382960fdb351e32ff39cf3bd061c3045ff

root@tmax-22cf9064:~/temp# 
root@tmax-22cf9064:~/temp# 
root@tmax-22cf9064:~/temp# docker ps | grep ubuntu
0ced7a4937e3        git-ubuntu:v1       "/bin/bash"              11 seconds ago      Up 9 seconds                                                         infallible_nobel
root@tmax-22cf9064:~/temp# docker attach 0ced7a4937e3

root@0ced7a4937e3:/# git version
git version 2.25.1


root@0ced7a4937e3:/# apt-get install net-tools
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following NEW packages will be installed:

Ctrl + p + q 
root@ade6606f4d49:/# read escape sequence

root@tmax-22cf9064:~/temp# docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                            NAMES

ade6606f4d49        git-ubuntu:v1       "/bin/bash"              8 minutes ago       Up 8 minutes                                                         angry_hypatia

f28ce410da67        sonatype/nexus3     "sh -c ${SONATYPE_DI   3 months ago        Up 3 months         0.0.0.0:5000->5000/tcp, 0.0.0.0:8081->8081/tcp   nexus

root@tmax-22cf9064:~/temp# 

root@tmax-22cf9064:~/temp# 

root@tmax-22cf9064:~/temp# docker commit -p ade6606f4d49 git-ubuntu:v2

sha256:3762ccff5178ce2783f189ab6f2fb5ce4078f971fd9e38d8a8df8d1b6283e0e5


root@tmax-22cf9064:~# docker save git-ubuntu:v2 git-ubuntu.tar
cowardly refusing to save to a terminal. Use the -o flag or redirect
root@tmax-22cf9064:~# docker save git-ubuntu:v2 -o git-ubuntu.tar


[root@s18-3 ~]# docker load --input git-ubuntu.tar
d22cfd6a8b16: Loading layer [==================================================>]  75.22MB/75.22MB
132bcd1e0eb5: Loading layer [==================================================>]  1.011MB/1.011MB
cf0f3facc4a3: Loading layer [==================================================>]  15.36kB/15.36kB
544a70a875fc: Loading layer [==================================================>]  3.072kB/3.072kB
18b871b7021a: Loading layer [==================================================>]   22.3MB/22.3MB
652fb87381de: Loading layer [==================================================>]  104.2MB/104.2MB
b4b4e7fb97cd: Loading layer [==================================================>]   1.65MB/1.65MB
Loaded image: git-ubuntu:v2


git config --global user.name "본인 이름"
git config --global user.email "본인 이메일 주소"

$ git config --local user.name "stunstunstun"
$ git config --local user.email "wjdsupj@gmail.com"

root@9efa9e37b2fe:/project# pwd
/project
root@9efa9e37b2fe:/project# $ git init


root@9efa9e37b2fe:/project# git commit -m "first change"
[master (root-commit) 4a21f37] first change
 1 file changed, 2 insertions(+)
 create mode 100644 license
root@9efa9e37b2fe:/project# 
root@9efa9e37b2fe:/project# git status
On branch master
nothing to commit, working tree clean


root@9efa9e37b2fe:/project# git config --global --list
user.email=tmaxanc@tmax.co.kr
user.name=tmaxanc


프로토콜
Git은 Local, HTTP, SSH, Git 이렇게 네 가지의 프로토콜을 사용할 수 있다. 이 절에서는 각각 어떤 경우에 유용한지 살펴본다.

Local
$ git clone /srv/git/project.git
아래처럼도 가능하다:
$ git clone file:///srv/git/project.git

remote
$ git remote add local_proj /srv/git/project.git


HTTP 프로토콜
마트 HTTP 프로토콜은 SSH나 Git 프로토콜처럼 통신한다. 다만 HTTP나 HTTPS 포트를 이용해 통신하고 다양한 HTTP 인증 방식을 사용한다는 것이 다르다. SSH는 키를 발급하고 관리해야 하는 번거로움이 있지만, HTTP는 사용자이름과 암호만으로 인증할 수 있기 때문에 더 편리하게 사용할 수 있다.

아마 지금은 Git에서 가장 많이 사용하는 프로토콜일 것이다. git:// 프로토콜처럼 익명으로 사용할 수도 있고, SSH처럼 인증을 거쳐 Push 할 수도 있기 때문이다. 이 두 가지 동작을 다른 URL로 나눌 필요없이 하나의 URL로 통합해서 사용할 수 있다. 그냥 인증기능을 켜놓은 저장소에 Push를 하면 서버는 사용자이름과 암호를 물어본다. 그리고 Fetch나 Pull 같은 읽기 작업에서도 같은 URL을 사용한다.

실제로 GitHub 같은 서비스에서 제공하는 저장소는 Clone을 할 때나 Push를 할 때 같은 URL을 사용한다. (예, https://github.com/schacon/simplegit)

Git 프로토콜




