---
layout: post
title: "주로 사용하는 git command 정리 1"
date: "2020-10-07 04:06:00"
author: Haebin Seo
categories: git
tags: git-command
---
# 주로 사용하는 Git command 정리 1

모든 작업의 시작과 끝은 git init, push로 끝날 만큼 git command를 많이 사용하지만 자주 사용하는 몇몇 명령어를 제외하고는 매번 10분 이상 구글링해 사용하는 편이다.
그래서 리눅스 커맨드를 정리하는 김에 주로 사용하거나 찾아본 적 있는 git 명령어들을 기록하고자 한다.

#### 1. git config
  - git 환경 설정
  - git은 버전을 저장할 떄 사용자 정보도 함께 저장하는데, 그 사용자를 등록할 수 있는 명령어이다.
  - github에 올렸을 때 commit의 작성자와 branch가 원격 저장소의 소유자, default branch와 일치하지 않으면 contribution으로 인정하지 않으므로, github 페이지의 이쁜 잔디밭을 원한다면 꼭 설정도록 하자.
  ```shell
  $ git config [--global] user.name "haebinseo" # [현재 컴퓨터의 모든 저장소에서] 사용자 이름 등록
  $ git config [--global] user.email "haebin.dev@gmail.com" # 사용자 이메일 등록
  ```

#### 2. git init
  - initialize
  - 현재 위치에 지역 저장소 생성
  ```shell
  $ git init
  ```

#### 3. git status
  - git 상태를 확인
  ```shell
  $ git status
  ```

#### 4. git log
  - commit 정보를 확인
  ```shell
  $ git init
  $ git init -(n) # 최근 n개의 commit 정보 출력
  $ git init --stat # commit 정보와 status를 함께 출력
  $ git init --branches # 모든 브랜치들의 commit 정보를 출력
  $ git init --graph # commit 정보를 그래프로 출력
  $ git init --oneline # 각 commit을 한 줄로 출력
  $ git init -p # 각 commit 정보와 diff 결과 출력
  $ git log --pretty=format:"%h - %an, %ar : %s" # format 옵션으로 결과 출력 지정
  # ca82a6d - Scott Chacon, 11 months ago : changed the version number
  # 085bb3b - Scott Chacon, 11 months ago : removed unnecessary test code
  # a11bef0 - Scott Chacon, 11 months ago : first commit
  ```

#### 5. git add
  - 파일을 작업트리에서 스테이지로 올린다 (.git/index)
  ```shell
  $ git add file1 # file1을 스테이지로 올림
  $ git add . # 작업트리에서 수정한 모든 파일을 스테이지로 올림
  ```

#### 6. git commit
  - 스테이지에 대기하던 파일들을 저장소에 저장하며 새로운 버전을 생성한다.
  ```shell
  $ git commit # 기본 에디터가 열리며 commit 메시지를 작성. 이후 commit이 생성
  $ git commit -m "{commit message}" # 한 줄의 커밋 메시지를 바로 작성하며 commit 생성
  $ git commit -am "{commit message}" # tracked file들의 변경사항을 스테이지에 올리며 동시에 commit. 
  $ git commit --amend # 에디터에서 마지막 commit message 수정. 이전 commit에 현재 변경 내용을 추가
  ```

#### 6. git checkout
  - Switch branches or restore working tree files
  ```shell
  $ git checkout # 기본 에디터가 열리며 commit 메시지를 작성. 이후 commit이 생성
  $ git checkout -m "{commit message}" # 한 줄의 커밋 메시지를 바로 작성하며 commit 생성
  $ git checkout -am "{commit message}" # tracked file들의 변경사항을 스테이지에 올리며 동시에 commit. 
  $ git checkout --amend # 에디터에서 마지막 commit message 수정. 이전 commit에 현재 변경 내용을 추가
  ```



