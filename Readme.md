



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



