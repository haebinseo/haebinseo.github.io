---
layout: post
title: "코딩테스트 개념 정리 - Hash"
date: "2020-11-05 23:53:00"
author: Haebin Seo
categories: codingtest
tags: codingtest summary hash
use_math: true
---
# 코딩테스트 개념 정리 - Hash
- `출제 빈도` - **높음** /  `평균 점수` - 보통

- Key-value쌍으로 데이터를 저장하는 자료구조
  <img src="/assets/codingTest/hash.png" alt="hash" style="width:480px;">      
  일반적으로 hash table의 삽입, 탐색, 삭제의 time complexity는 O(1)이지만 해시 충돌이 발생하는 최악의 경우 O(n)이 될 수 있다.

- 보통 라이브러리나 언어 자체에 내장 구현되어있는 map 자료형을 사용한다.

- 해시 충돌 (Hash Collision)  
  무한한 값을 유한한 값으로 표현하다 보면 서로 다른 두 개 이상의 입력이 동일한 출력을 가지는 일이 발생한다. 해시 충돌은 서로 다른 복수의 key들이 같은 hash 값으로 mapping되어 충돌을 일으키는 현상을 의미한다.

  해결 방법
  - **Separate Chaining**  
    한 버킷당 들어갈 수 있는 엔트리의 수에 제한을 두지 않음으로써 모든 자료를 해시테이블에 담는 기법이다. 연결리스트(Linked List) 자료구조를 이용해 중복 hash 값을 가지는 자료를 기존의 자료 다음에 위치시킨다.
    <img src="/assets/codingTest/separateChaining.png" alt="separateChaining" style="width:480px;">      
    장점:
    1. 한정된 저장소(Bucket)을 효율적으로 사용할 수 있다.
    2. 해시 함수(Hash Function)을 선택하는 중요성이 상대적으로 적다.
    3. 상대적으로 적은 메모리를 사용한다. 미리 공간을 잡아 놓을 필요가 없다.

    단점:
    1. 한 Hash에 자료들이 계속 연결된다면(쏠림 현상) 검색 효율을 낮출 수 있다.
    2. 외부 저장 공간을 사용한다.
    3. 외부 저장 공간 작업을 추가로 해야 한다.

    time complexity:  
    해시 테이블의 저장소(Bucket)의 길이를 $n$, 키(key)의 수를 $m$이라 할 때, 해시함수가 키들을 모든 저장소에 균등하게 할당한다고 가정하자(simple uniform hashing). 이 경우, 1개의 hash 값에는 $\frac m n$개의 키가 할당된다. 이 값을 $α$라고 하자. ($α = \frac m n$)

    |           |    Average    | Worst  |
    | :-------: | :-----------: | :----: |
    | Insertion | $O(1)$ or $O(α)$ | $O(n)$ |
    |  Search   |    $O(α)$     | $O(n)$ |
    | Deletion  |    $O(α)$     | $O(n)$ |

    head에 삽입시에는 O(1), tail에 삽입시에는 $O(α)$의 시간 복잡도를 가진다.
    최악의 경우는 한 hash에 모든 key가 몰릴 때이다.

  - **Open Addressing** (개방주소법)  
    chaining과 달리 한 버킷당 들어갈 수 있는 엔트리가 하나뿐인 해시테이블로, 해시 충돌시 해시함수로 얻은 주소가 아닌 비어있는 다른 주소를 찾아 데이터를 저장하는 기법이다.
    <img src="/assets/codingTest/openAddressing.png" alt="openAddressing" style="width:400px;">
    위의 그림에서는 해시가 충돌할 경우 1씩 더하며 비어있는 주소를 탐색한다. 특정 해시값에 키가 몰리게 되면 비어있는 주소를 탐색해야 하는 open addressing 특성상 효율성이 크게 떨어지게 되는데, 이는 탐사(probing) 방식으로 어느정도 해결할 수 있다.

    - 선형 탐색(Linear Probing):  
      다음 해시(+1)나 n개(+n)를 건너뛰어 비어있는 해시에 데이터를 저장한다.
      탐사 이동폭이 고정되어 있으므로 특정 해시값 주변 버킷이 모두 채워져 있는 `primary clustring` 문제에 취약하다.
      <img src="/assets/codingTest/linearProbing.png" alt="linearProbing" style="width:180px;">

    - 제곱 탐색(Quadratic Probing):  
      고정 폭으로 이동하는 선형 탐사와 달리 폭이 제곱수로 늘어난다. 예를 들어 임의의 키값에 해당하는 데이터에 액세스할 때 충돌이 일어나면 1^2^칸, 여기에서도 충돌이 일어나면 2^2^칸, 그 다음엔 3^2^칸 옮기는 식으로 탐색한다.
      하지만 초기 해시값이 같으면 다음 탐사 위치 또한 동일하기 때문에 효율성이 떨어지게 된다. 때문에 여러 개의 서로 다른 키들이 동일한 초기 해시값을 갖는 `secondary clustering`에 취약하다.
      <img src="/assets/codingTest/quadraticProbing.png" alt="quadraticProbing" style="width:240px;">

    - 이중 해시(Double Hashing):  
      2개의 해시함수를 사용해서 해시값의 규칙성을 없애 clustering을 방지한다. 2개의 해시함수 중 하나는 최초의 해시값을 얻을 때, 다른 하나는 해시충돌이 일어났을 때 탐사 이동폭을 얻기 위해 사용한다. 이렇게 되면 최초 해시값이 같더라도 탐사 이동폭이 달라지고, 탐사 이동폭이 같더라도 최초 해시값이 달라지므로 `primary clustering`와 `secondary clustering`를 모두 완화시킬 수 있다.
      <img src="/assets/codingTest/doubleHashing.png" alt="doubleHashing" style="width:480px;">

    time complexity:  
    chaining처럼 키들이 균등하게 할당된다고 가정하자. open addressing은 해시값 중복을 허용하지 않으므로, Load factor $α$는 1과 같거나 작다고 가정한다. 즉, 해시테이블에 데이터가 꽉 차지 않음을 전제로 한다.
    open addressing 탐색의 계산복잡성은 탐사 횟수에 비례하는데, 탐사 횟수의 기댓값은 $\frac{1}{1-α}$이므로 평균적인 시간 복잡도는 $O(\frac{1}{1-α})$, 최악의 경우에는 $O(n)$이 된다. 

- 참조
  - hash 개념: 
    - [https://velog.io/@cyranocoding/Hash-Hashing-Hash-Table해시-해싱-해시테이블-자료구조의-이해-6ijyonph6o](https://velog.io/@cyranocoding/Hash-Hashing-Hash-Table%ED%95%B4%EC%8B%9C-%ED%95%B4%EC%8B%B1-%ED%95%B4%EC%8B%9C%ED%85%8C%EC%9D%B4%EB%B8%94-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%EC%9D%98-%EC%9D%B4%ED%95%B4-6ijyonph6o)
    - [https://ratsgo.github.io/data%20structure&algorithm/2017/10/25/hash/](https://ratsgo.github.io/data%20structure&algorithm/2017/10/25/hash/)