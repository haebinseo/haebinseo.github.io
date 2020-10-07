---
layout: post
title: "주로 사용하는 Linux command 정리 1"
date: "2020-10-07 04:06:00"
author: Haebin Seo
categories: linux
tags: linux-command
---
# 주로 사용하는 Linux command 정리 1

노트북에 Ubuntu를 설치해 사용한지 반년이 다 되어가지만 가끔 사용하는 명령어들은 아직도 구글링해가며 사용하곤 한다. 기억이 나긴하지만 확신을 못해서 찾아보고 사용하게 되는 것 같다.
그래서 이번 기회에 주로 사용하는 명령어 위주로 간단히 정리하려고 한다.

#### 1. cd
  - Change directory
  - 매우 자주 사용하므로 간단한 예시만을 남긴다.
  ```shell
  $ cd .. # 상위 폴더로 이동
  $ cd - # 이전 작업 폴더로 이동
  $ cd / # root 경로로 이동
  $ cd ~ # Home 경로로 이동
  ```

#### 2. pwd
  - Print working directory
  - 현재 작업 중인 디렉토리의 절대 경로
  ```shell
  $ pwd
  /home/havebeen/repos/
  ```

#### 3. ls
  - List
  - 디렉토리 내용 출력
  ```shell
  $ ls # 현재 디렉토리 내용 출력
  $ ls -a # --all / 모든 내용
  $ ls -l # long / 자세한 내용(권한, 포함된 파일 수, 소유자, 그룹, 크기, 수정일자, 파일이름)
  $ ls -S # Size / 파일 크기순 정렬
  $ ls -r # --reverse / 거꾸로 출력(default - alphabetical)
  $ ls -R # --recursive / 하위 디렉토리 내용까지
  $ ls -h # --human-readable / 파일크기를 K, M, G 단위로 보기 좋게 출력
  
  # ls -l은 default로 mtime(수정 시간)을 출력한다.
  $ ls -lu # atime(접근 시간) 출력
  $ ls -lc # ctime(변경 시간) 출력
  ```

#### 4. stat
  - Status
  - 파일 상태 정보 출력
  ```shell
  $ stat package.json
    File: package.json
    Size: 1397      	Blocks: 8          IO Block: 4096   일반 파일
  Device: 10305h/66309d	Inode: 1184675     Links: 1
  Access: (0644/-rw-r--r--)  Uid: ( 1000/havebeen)   Gid: ( 1000/havebeen)
  Access: 2020-10-06 19:49:58.475783679 +0900
  Modify: 2020-09-26 23:03:03.593015061 +0900
  Change: 2020-09-26 23:03:03.593015061 +0900
   Birth: -
  ```

#### 5. man
  - Manual
  - 명령어, 프로그램의 사용법 출력
  - man 사용법
    |            key            |        의미        |
    | :-----------------------: | :----------------: |
    |             q             |   quit / 나가기    |
    |             h             | help / 사용법 확인 |
    |        ←, →, ↑, ↓         |   한줄씩 넘기기    |
    | Page Up / Down, Space bar | 한페이지씩 넘기기  |
    |    / + 검색어 + enter     |        검색        |
    |             n             |     다음 위치      |
    |             N             |     이전 문자      |
  ```shell
  $ man rm # rm명령어의 메뉴얼 출력
  ```

#### 6. mkdir / rmdir
  - Make directories / Remove empty directories
  ```shell
  $ mkdir dir1 # 현재 디렉토리에 dir1 디렉토리 생성
  $ mkdir dir1 dir2 # 한 번에 여러 디렉토리 생성
  $ mkdir -p dir1/dir2 # --parents / 경로 중에 존재하지 않는 디렉토리까지 생성
  $ mkdir -m 700 dir1 # --mode=700 / 디렉토리를 생성하며 권한도 지정
  --------------------------------------------------------
  $ rmdir dir1 # 현재 디렉토리에 dir1 디렉토리 삭제
  $ rmdir dir1 dir2 # 한 번에 여러 디렉토리 삭제
  $ rmdir -p dir1/dir2 # --parents / 경로 중의 디렉토리들도 같이 삭제
    # 만약 다른 디렉토리나 파일이 존재하면 해당 상위 디렉토리는 제외
  ```

#### 7. cp
  - Copy
  ```shell
  $ cp file1 file2 # file1을 복사하여 file2로 저장
  $ cp file1 dir1/ # file1을 복사하여 dir1 안에 저장
  $ cp file1 file2 dir1/ # 한 번에 여러 파일을 복사하여 dir1에 저장
  $ cp -r dir1/ dir2/ # --recursive / 디렉토리 dir1을 전체 복사하여 dir2로 저장
  $ cp -r dir1/ backup$(date '+_%Y%m%d') # date 명령어와 조합하여 쉽게 백업
    # ex) backup_20201007
  ```

#### 8. mv
  - Move
  ```shell
  $ mv file1 file2 # file1을 file2로 이름 변경
  $ mv file1 dir1/ # file1을 dir1로 이동
  $ mv file1 file2 dir1/ # 한 번에 여러 파일을 dir1로 이동
  $ mv dir1/ dir2/ # 디렉토리 dir1을 dir2로 이름 변경
  ```

#### 9. rm
  - Remove
  ```shell
  $ rm file1 # file1 삭제
  $ rm *.txt # '.txt'로 끝나는 파일을 모두 삭제
  $ rm * # 현재 디렉토리의 모든 파일 삭제
  $ rm -r dir1/ # --recursive / 디렉토리 dir1 삭제
  $ rm -rf dir1/ # --recursive --force / 디렉토리 dir1 강제 삭제
  $ rm -ri dir1/ # --recursive --interactive / 디렉토리의 내용물을 삭제할 때마다 사용자에게 질문
  ```
