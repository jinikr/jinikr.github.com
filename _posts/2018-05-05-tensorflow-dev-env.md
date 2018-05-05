---
layout: post
title: "tensorflow 개발환경 만들기 (docker 사용)"
author: yjk
categories: tensorflow
---

### docker 이미지
  - tensorflow/tensorflow:latest-py3  
    python3 사용
    <https://hub.docker.com/r/tensorflow/tensorflow/>
  - [docker를 사용한 golang 개발환경 만들기 참고](https://jinikr.github.io/2018-05-05/golang-dev-env)

### hello world
  ```sh
  # 현재 디렉토리에 example download
  $ git clone https://github.com/jinikr/tensorflow-excersice .
  Cloning into '.'...
  remote: Counting objects: 6, done.
  remote: Compressing objects: 100% (4/4), done.
  remote: Total 6 (delta 0), reused 6 (delta 0), pack-reused 0
  Unpacking objects: 100% (6/6), done.

  # docker 실행 (docker-compose 사용)
  $ docker-compose up -d
  Creating network "tensorflow-excersice_default" with the default driver
  Building golang-dev
  Step 1/2 : FROM tensorflow/tensorflow:latest-py3
  latest-py3: Pulling from tensorflow/tensorflow
  297061f60c36: Pull complete
  e9ccef17b516: Pull complete
  dbc33716854d: Pull complete
  8fe36b178d25: Pull complete
  686596545a94: Pull complete
  11251ff8ca17: Pull complete
  632ad5221740: Pull complete
  c61bc0eaf8d9: Pull complete
  f5e02d7ae67a: Pull complete
  f29a5775b72a: Pull complete
  c9bf16bc9d22: Pull complete
  d4423e297b68: Pull complete
  418a4217796d: Pull complete
  02b18d984060: Pull complete
  Digest: sha256:24b533d6db08369375f85b7d8da0e7d8c35c105337a7f6fa71947195b900eec9
  Status: Downloaded newer image for tensorflow/tensorflow:latest-py3
  ---> a83a3dd79ff9
  Step 2/2 : RUN pip install "h5py==2.8.0rc1"     && export TF_CPP_MIN_LOG_LEVEL=2
  ---> Running in 7d944f6ed0e3
  Collecting h5py==2.8.0rc1
    Downloading https://files.pythonhosted.org/packages/00/26/4bbf836d3a32d91b0bec3a7533d27abcd13aeafb68fc7cbfdf3ff792758b/h5py-2.8.0rc1-cp35-cp35m-manylinux1_x86_64.whl (2.4MB)
  ^@Requirement already satisfied: numpy>=1.7 in /usr/local/lib/python3.5/dist-packages (from h5py==2.8.0rc1) (1.14.2)
  Requirement already satisfied: six in /usr/local/lib/python3.5/dist-packages (from h5py==2.8.0rc1) (1.11.0)
  Installing collected packages: h5py
    Found existing installation: h5py 2.7.1
      Uninstalling h5py-2.7.1:
        Successfully uninstalled h5py-2.7.1
  Successfully installed h5py-2.8.0rc1
  Removing intermediate container 7d944f6ed0e3
  ---> 57bc8e411f5d
  Successfully built 57bc8e411f5d
  Successfully tagged tf-dev:0.1
  WARNING: Image for service golang-dev was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
  Creating tf-dev ... done

  # docker 컨테이너 확인
  $ docker ps
  CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                NAMES
  95fa37fdd8a4        tf-dev:0.1              "/run_jupyter.sh --a…"   About an hour ago   Up About an hour    6006/tcp, 8888/tcp   tf-dev

  # docker 컨테이너의 bash 접속
  $ docker exec -it tf-dev bash
  root@95fa37fdd8a4:/var/app#

  # hello world 테스트
  root@95fa37fdd8a4:/var/app# cd 01.\ hello-world/
  root@95fa37fdd8a4:/var/app/01. hello-world# python hello-world.py
  Tensor("Const:0", shape=(), dtype=string)
  2018-05-05 12:11:27.978539: I tensorflow/core/platform/cpu_feature_guard.cc:140] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX2 FMA
  b'Hello, Tensor.'
  ```

### 부록
  - 경고메세지 보기싫으면..
    ```sh
    export TF_CPP_MIN_LOG_LEVEL=2
    ```
