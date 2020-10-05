---
layout: post
title: "Github에 ssh-key 등록해 사용하기"
date: "2020-10-05 23:44:00"
author: Haebin Seo
categories: git
tags: git ssh-key
---
# Github에 ssh-key 등록해 사용하기

그 동안 git을 push, pull할 떄 vscode의 git authentication 설정으로 로그인 없이 사용했었는데, 이번에 깃헙 계정을 새로 만들면서 git config --global의 이름과 이메일을 변경해도 이전 계정으로 접근이 시도되며 403 error를 뱉는 일을 겪었다.
검색하면 나오는 결과라고는 windows 제어판에서 자격증명을 지우라는 것 뿐이었고... 나는 우분투였다ㅠㅠㅠ 암호 및 키에 등록되어있는 git 계정을 삭제했지만 역시 해결되지 않았고 결국 해결하지 못한 채 ssh-key를 등록해 사용하는 방향으로 틀었다.

ssh-key를 사용해 로그인없이 github을 사용하는 것이 이번이 처음은 아니다. 전에 django서버를 aws ec2 서버로 돌릴 때 사용했었는데, 몇 달이 지난 지금 벌써 기억이 가물가물해 다시 구글링해 적용하게 되었다.

이번 포스팅은 이 내용을 나중에 최소한의 시간으로 다시 찾고자 남기는 기록이다.

- SSH (Secure Shell Protocol)
  - 암호화된 원격 접속 프로토콜
  - 컴퓨터가 공용 네트워크를 통해 통신할 때 보안적으로 안전하게 통신하기 위해 사용하는 접속 프로토콜이다
- SSH Key
  - SSH로 서버에 접속 시 서버에 비밀번호 대신 key를 제출하는 방식이다.

### 1. SSH Key 파일 확인
  - 일반적으로 SSH Key는 ~/.ssh 경로에 존재한다.
  - 기존 생성해 놓은 키가 있다면 .pub 파일등과 같은 키 파일이 있을 것이다.
  - 혹시 디렉토리가 없다면 다음의 명령어로 생성하자.
    ```shell
    $ mkdir ~/.ssh
    $ chmod 700 ~/.ssh
    $ cd ~/.ssh
    ```

### 2. SSH Key 생성하기
  - ssh-keygen을 사용해 key를 생성한다.
    ```shell
    $ ssh-keygen -t rsa -b 4096 -C "yourEmail@example.com"
    ```
    cf ) -t rsa는 rsa라는 암호화 방식으로 키를 생성한다는 의미다.
    SSH 키는 크기가 2048비트 또는 4096비트인 RSA 키여야 한다. 이 경우에는 4096으로 지정하였다.
  - 다음은 경로지정이다. 필요한 경우에만 절대 경로로 입력해주자.
    ```shell
    $ Generating public/private rsa key pair.
    $ Enter file in which to save the key (/home/user/.ssh/id_rsa):
    ```
  - 암호(Passphrase)설정도 필요하면 입력해주자.
    ```shell
    $ Enter passphrase (empty for no passphrase):
    $ Enter same passphrase again:
    ```
    암호를 설정했다면 ssh-agent에 키를 등록해두면 매번 키를 입력하지 않아도 되므로 편하다.
    ```shell
    $ eval `ssh-agent -s` # 백그라운드 실행
    $ ssh-add ~/.ssh/id_rsa # 키 등록
    ```
  - 생성된 결과물은 다음과 같다.
    - id_rsa  
      private key. 본인의 컴퓨터에 저장되며, 이 key를 통해 암호화된 메시지를 복호화할 수 있다.

    - id_rsa.pub  
      public key. 공개되어도 비교적 안전한 Key이다. Public Key를 통해 메시지를 전송하기 전 암호화를 하게된다.
      public key로는 복호화는 불가능하고, 접속하려는 remote machine의 authorized_keys에 입력하여 사용한다.

### 3. Github에 공개키 등록하기
  - 특정 프로젝트 혹은 계정의 설정에서 등록하면 된다. 여기서는 계정의 SSH Key로 등록했다.
  - Title: 사용자 지정 키 이름
    Key: 공개 키 내용
  - 키 내용 확인방법 
    ```shell
    $ cat ~/.ssh/id_rsa.pub 
    $ xclip -selection clipboard < ~/.ssh/id_rsa.pub # Mac OS면 pbcopy를 쓰자
    ```

### 4. Github 설정 use SSH로 변경
  - 제일 중요하다(...)
  - https로 클론하고 왜 안되는지 고민하는 일이 없도록 하자
  - 하지만 이미 클론한 상태라면? 다시 클론해야 되나...?ㅠㅠㅠ
  아니다! remote를 변경해서 해결해주자 :).
    ```shell
    $ git remote remove origin
    $ git remote add origin git@github.com:[계정명]/[저장소명].git
    $ git remote show # origin이 잘 등록되었는지 확인
    ```

### 5. 끝!
  - 고생했다. 이제 git push하고 쉬자!
