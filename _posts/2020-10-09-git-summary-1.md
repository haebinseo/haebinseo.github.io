---
layout: post
title: "git 개념 정리 1"
date: "2020-10-09 03:55:00"
author: Haebin Seo
categories: git
tags: git-study
---
#### Git
- Git은 파일을 Committed, Modified, Staged 이렇게 세 가지 상태로 관리한다.
  - Committed : 데이터가 로컬 데이터베이스에 안전하게 저장됐다는 것을 의미한다.
  - Modified : 수정한 파일을 아직 Staging Area에 추가하지 않았음을 의미한다.
  - Staged : 현재 수정한 파일을 곧 커밋할 것이라고 표시한 상태를 의미한다.
- 이 세 가지 상태는 Git 프로젝트의 세 가지 단계와 연결돼 있다.
  ![areas](/assets/git/areas.png)
  - **Git directory**는 Git이 프로젝트의 메타데이터와 객체 데이터베이스를 저장하는 곳을 말한다. 이 Git directory가 Git의 핵심이다. 다른 컴퓨터에 있는 저장소를 Clone 할 때 Git directory가 만들어진다.
  - **Working tree**는 프로젝트의 특정 버전을 Checkout 한 것이다. Git directory는 지금 작업하는 디스크에 있고 그 directory 안에 압축된 데이터베이스에서 파일을 가져와서 working tree를 만든다.
  - **Staging Area**는 Git directory에 있다. 단순한 파일이고 곧 커밋할 파일에 대한 정보를 저장한다. Git에서는 기술용어로는 “Index” 라고 하지만, “Staging Area” 라는 용어를 써도 상관 없다.
  - Staging Area와 Git directory는 눈에 보이지 않는다. Staging Area 내용는 .git/index에, Git directory 내용은 .git/HEAD 파일에 저장된다.
- 작업트리의 파일은 크게 **tracked** 상태와 **untracked** 상태로 나뉜다.
  - 한번이라도 commit한 파일은 수정 여부를 계속 추적한다.
    ![lifeCycle](/assets/git/lifecycle.png)

#### Revision
- 특정 git object, 즉 commit을 가리킬 수 있는 표현식을 의미한다.
- ex) `HEAD~2`, `master`, `a8dd808db6d87a1d809b1a223e08ab69602b2d3a`, `HEAD:test.txt`
- Git은 40글자의 해시 값의 앞 몇 글자만으로도 어떤 커밋인지 충분히 식별할 수 있다. 저장소 안에서 해시 값이 중복되지 않으면 해시 값의 앞 4자만으로도 나타낼 수 있다. `git log`에 `--abbrev-commit` 옵션을 추가하면 짧고 중복되지 않는 해시 값을 보여준다. 기본으로 7자를 보여주고 해시 값이 중복되는 경우 더 긴 해시 값을 보여준다.
- RefLog
  reflog의 일은 모두 로컬의 일이므로 내 Reflog가 동료의 저장소에는 있을 수 없다. 즉, 5분 전에 Clone 한 저장소에 사용하면 아무 결과도 나오지 않는다.

  |        rule         |                의미                 |        예시        |
  | :-----------------: | :---------------------------------: | :----------------: |
  |  &lt;refname>@{n}   | &lt;refname>가 n번 전에 가리켰던 것 | HEAD@{5}, main@{2} |
  | &lt;refname>@{date} | &lt;refname>가 {date}에 가리켰던 것 |  HEAD@{yesterday}  |

- 계통 관계
  HEAD^ 는 바로 “HEAD의 부모” 를 의미하므로 HEAD 바로 이전 커밋을 보여준다.

  |     rule      |        의미         | 예시  |
  | :-----------: | :-----------------: | :---: |
  | &lt;refname>^ | &lt;refname>의 부모 | HEAD^ |
  | &lt;refname>^n | &lt;refname>의 n번째 부모<br>(merge commit에서만 가능) | HEAD^2 |
  | &lt;refname>~ | &lt;refname>의 부모 | HEAD~ |
  | &lt;refname>~n | &lt;refname>의 n번째 조상 | HEAD~2 (== HEAD^^) |

- Double Dot
  - 어떤 커밋들이 한쪽에는 관련됐고 다른 쪽에는 관련되지 않았는지 Git에게 물어보는 것이다.
  - 다음과 같은 commit history가 있다고 가정하자
    ![double-dot](/assets/git/double-dot.png)
    ```shell
    $ git log master..experiment # master에는 없지만, experiment에는 있는 커밋
    D
    C

    $ git log experiment..master # experiment 에는 없고 master 에만 있는 커밋
    F
    E
    ```
- Triple Dot
  - 양쪽에 있는 두 Refs 사이에서 공통으로 가지는 것을 제외하고 서로 다른 커밋만 보여준다.
    ```shell
    $ git log master...experiment
    F
    E
    D
    C

    $ git log master...experiment --left-right # 각 커밋이 어느 브랜치에 속하는지 보여줌
    < F
    < E
    > D
    > C
    ```

#### HEAD
  - 보통 branch는 특정 commit의 revision number를 가리키고 HEAD가 이 branch를 가리킨다.
  - 이렇게 HEAD -> branch -> commit 순으로 commit을 가리키는 상태(state)를 **_Attached HEAD_**, 브랜치를 통하지 않고 HEAD가 커밋을 직접 가리키는 상태는 **_Detached HEAD_**라고 한다.
  - Attached HEAD state에서는 새로운 commit을 작성했을 때, HEAD가 자동으로 최근 commit을 따라간다. 하지만 Detached HEAD state에서는 따라가지 않으므로, 이후 다른 branch나 commit으로 chackout했을 때 이 commit을 찾기가 매우 힘들어진다.