---
layout: post
title: "golang 개발환경 만들기 (docker 사용)"
author: yjk
categories: golang
---

### docker 설치.
  - 커뮤니티에디션 설치
  - <https://www.docker.com/community-edition#/download>

### docker 이미지 고르기
  - <https://hub.docker.com/_/golang/>  
  - stretch(Debian 9) 을 사용하자.  
    golang:1.10.1-stretch
  - alpine 계열은 가볍고... 가볍지만, 개발환경으로 사용하기엔 불편.  
    패키지관리자도 익숙하지 않고, bash도 없고.. 물론 설치하고 익숙해지면 되지만, 일단 익숙한걸로 사용하자.  
    <https://www.lesstif.com/pages/viewpage.action?pageId=35356819>

### docker 실행하기
  - 이미지를 받아서 실행하자.
    ```sh
    $ docker pull golang:1.10.1-stretch
    $ docker run -w /var/app -v `pwd`:/var/app --name golang-dev -i -t golang:1.10.1-stretch bash
    ```
  - 컨테이너 삭제 명령
    ```sh
    $ docker rm golang-dev
    ```

### docker-compose 활용
  - docker-compose를 사용하자 (docker에 포함되어 이미 설치되어 있을것임.)  
    <https://docs.docker.com/compose/>

  - 실행
    ```sh
    $ docker-compose up -d
    ```
  - docker-compose로 실행된 docker container에 접속하자.
    ```sh
    $ docker exec -it golang-dev bash
    ```
  - 종료
    ```sh
    $ docker-compose down
    ```

### hello world
  ```sh
  # example download
  $ git clone https://github.com/jinikr/golang-exercise .
  Cloning into '.'...
  remote: Counting objects: 13, done.
  remote: Compressing objects: 100% (7/7), done.
  remote: Total 13 (delta 0), reused 7 (delta 0), pack-reused 0
  Unpacking objects: 100% (13/13), done.

  # docker 실행 (docker-compose 사용)
  $ docker-compose up -d
  Pulling golang-dev (golang:1.10.1-stretch)...
  1.10.1-stretch: Pulling from library/golang
  cc1a78bfd46b: Pull complete
  53c14872d997: Pull complete
  99ae159b9cae: Pull complete
  66cbf2b79699: Pull complete
  a16f46d95485: Pull complete
  f235c5ef7891: Pull complete
  cdd98e20e590: Pull complete
  Digest: sha256:e7331dea607f66adb0d597f43d7cb9cdc36ac73cff56536bde3311b46c5186e9
  Status: Downloaded newer image for golang:1.10.1-stretch
  Creating golang-dev ... done

  # docker 컨테이너 확인
  $ docker ps
  CONTAINER ID        IMAGE                   COMMAND             CREATED             STATUS              PORTS               NAMES
  5a40fb644fbd        golang:1.10.1-stretch   "bash"              43 seconds ago      Up 42 seconds                           golang-dev

  # docker 컨테이너의 bash 접속
  $ docker exec -it golang-dev bash
  root@5a40fb644fbd:/var/app#

  # hello world 테스트
  root@5a40fb644fbd:/var/app# cd 01.\ hello-world/
  root@5a40fb644fbd:/var/app/01. hello-world# go run hello-world.go
  hello, world
  ```

### 부록
  - docker cli 관련  
    <https://docs.docker.com/engine/reference/commandline/cli/>
    ```sh
    # docker container 조회
    $ docker ps
    # 종료된 컨테이너도 확인하려면 -a 옵션을 붙인다.)
    $ docker ps -a

    # docker image 확인
    $ docker images

    # docker image 삭제
    $ docker rmi {image name}

    # 미사용 이미지 정리
    $ docker image prune
    ```
