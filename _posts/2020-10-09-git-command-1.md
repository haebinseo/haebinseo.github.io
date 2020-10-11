---
layout: post
title: "주로 사용하는 git command 정리 1"
date: "2020-10-09 05:07:00"
author: Haebin Seo
categories: git
tags: git-command
---
# 주로 사용하는 Git command 정리 1

모든 작업은 git pull, push로 끝날 만큼 git command를 많이 사용하지만, 자주 사용하는 몇몇 명령어를 제외하고는 매번 10분 이상 구글링해 사용하곤 한다. 그래서 리눅스 커맨드를 정리하는 김에 주로 사용하거나 찾아본 적 있는 git 명령어들을 기록하게 되었다.

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
  $ git log
  $ git log -(n) # 최근 n개의 commit 정보 출력
  $ git log --stat # commit 정보와 status를 함께 출력
  $ git log --branches # 모든 브랜치들의 commit 정보를 출력
  $ git log --graph # commit 정보를 그래프로 출력
  $ git log --oneline # 각 commit을 한 줄로 출력
  $ git log -p # 각 commit 정보와 diff 결과 출력
  $ git log --abbrev-commit # 짧고 중복되지 않는 해시 값으로 revision number 출력

  $ git log --pretty=format:"%h - %an, %ar : %s" # format 옵션으로 결과 출력 지정
  ca82a6d - Scott Chacon, 11 months ago : changed the version number
  085bb3b - Scott Chacon, 11 months ago : removed unnecessary test code
  a11bef0 - Scott Chacon, 11 months ago : first commit
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
  $ git commit --amend # 마지막 commit message 수정. 이전 commit에 현재 스테이지에 올라온 내용을 추가
  ```

#### 7. git checkout
- 브랜치 전환 혹은 작업트리의 파일 복구
  ```shell
  $ git checkout -- file1 # modified 상태의 파일을 다시 unmodified 상태로 되돌림. 
  $ git checkout {revision number} # 해당 commit으로 HEAD 이동 (detached HEAD)
  $ git checkout {branch name} # 해당 branch로 HEAD 이동
  $ git checkout -b {branch name} # branch를 생성하고 이동
  ```

#### 8. git reset
- 현재 HEAD를 특정 상태로 재설정
- reset의 단계
  1. HEAD 이동 (--soft 옵션은 여기까지 동작)
  2. Index 업데이트 (--mixed 옵션은 여기까지 동작 / default)
  3. Working tree 업데이트 (--hard 옵션은 여기까지 동작)
  
  ```shell
  $ git reset {revision number} # 해당 commit으로 HEAD 이동, HEAD 내용으로 Index 업데이트
  $ git reset {revision number} file1 # HEAD 이동 생략, 해당 commit의 file1으로 Index의 file1 업데이트
  ```

#### 9. git revert
- 특정 commit의 작업을 반대로 함(revert)
- reset과는 다르게 기존의 commit을 그대로 두고 새로운 commit을 작성한다.
  ```shell
  $ git revert {revision number} # 해당 commit의 작업 내역 revert
  $ git revert 2664ce8..15413dc # 15413dc부터 2664ce8 바로 이전 commit까지 revert
  ```
