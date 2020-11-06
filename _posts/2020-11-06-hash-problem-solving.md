---
layout: post
title: "hash 문제 풀이"
date: "2020-11-06 15:50:00"
author: Haebin Seo
categories: coding-test
tags: coding-test hash
use_math: true
---
문제 바로가기  
[`완주하지 못한 선수`](#완주하지-못한-선수) [`위장`](#위장) [`베스트앨범`](#베스트앨범)

### [완주하지 못한 선수](https://programmers.co.kr/learn/courses/30/lessons/42576?language=javascript)
`level 1`
##### 문제 설명
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

##### 제한사항
- completion의 길이는 participant의 길이보다 1 작습니다.
- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

<details>
<summary>입출력 예 보이기 / 숨기기</summary>
<div markdown="1">

##### 입출력 예

| participant                                       | completion                               | return   |
| ------------------------------------------------- | ---------------------------------------- | -------- |
| ["leo", "kiki", "eden"]                           | ["eden", "kiki"]                         | "leo"    |
| ["marina", "josipa", "nikola", "vinko", "filipa"] | ["josipa", "filipa", "marina", "nikola"] | "vinko"  |
| ["mislav", "stanko", "mislav", "ana"]             | ["stanko", "ana", "mislav"]              | "mislav" |

###### 입출력 예 설명
예제 #1
leo는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #2
vinko는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #3
mislav는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.
</div>
</details><br>

##### 문제 풀이
[`JavaScript`](#완주하지-못한-선수-javascript)

<h5 id="완주하지-못한-선수-javascript">JavaScript</h5>

```js
function solution(participant, completion) {
  // 완주자의 이름과 해당 이름의 완주자의 수를 key-value 쌍으로 하는 해시 테이블 생성  
  const countMap = new Map();
  for (let i = 0; i < completion.length; i++) {
    countMap.set(completion[i], (countMap.get(completion[i]) || 0) + 1);
  }
  // 해당 이름의 완주자의 수를 1씩 감소하며 완주자 명단에 없는 참가자 탐색.
  const result = participant.find((p) => {
    return !countMap.get(p) ? true : countMap.set(p, countMap.get(p) -1) && false;
  })
  
  return result;
}
```

---

### [위장](https://programmers.co.kr/learn/courses/30/lessons/42578?language=javascript)
`level 2`
##### 문제 설명
스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.

예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

| 종류  |            이름            |
| :---: | :------------------------: |
| 얼굴  | 동그란 안경, 검정 선글라스 |
| 상의  |       파란색 티셔츠        |
| 하의  |           청바지           |
| 겉옷  |          긴 코트           |

스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항
- clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
- 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
- 같은 이름을 가진 의상은 존재하지 않습니다.
- clothes의 모든 원소는 문자열로 이루어져 있습니다.
- 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.
- 스파이는 하루에 최소 한 개의 의상은 입습니다.

<details>
<summary>입출력 예 보이기 / 숨기기</summary>
<div markdown="1">

##### 입출력 예

| clothes                                                                                     | return |
| ------------------------------------------------------------------------------------------- | ------ |
| \[["yellow_hat", "headgear"], ["blue_sunglasses", "eyewear"], ["green_turban", "headgear"]] | 5      |
| \[["crow_mask", "face"], ["blue_sunglasses", "face"], ["smoky_makeup", "face"]]             | 3      |

###### 입출력 예 설명
예제 #1
headgear에 해당하는 의상이 yellow_hat, green_turban이고 eyewear에 해당하는 의상이 blue_sunglasses이므로 아래와 같이 5개의 조합이 가능합니다.
1. yellow_hat
2. blue_sunglasses
3. green_turban
4. yellow_hat + blue_sunglasses
5. green_turban + blue_sunglasses

예제 #2
face에 해당하는 의상이 crow_mask, blue_sunglasses, smoky_makeup이므로 아래와 같이 3개의 조합이 가능합니다.
1. crow_mask
2. blue_sunglasses
3. smoky_makeup
</div>
</details><br>

##### 문제 풀이
[`JavaScript`](#위장-javascript)

<h5 id="위장-javascript">JavaScript</h5>

```js
function solution(clothes) {
    let answer = 1;
    const clothMap = new Map();
    for (let i = 0; i < clothes.length; i++) {
      clothMap.set(clothes[i][1], (clothMap.get(clothes[i][1]) || 0) + 1);
    }
    for (let count of clothMap.values()) answer *= count + 1;
    return answer - 1;
}
```

---

### [베스트앨범](https://programmers.co.kr/learn/courses/30/lessons/42579?language=javascript)
`level 3`
##### 문제 설명
스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

##### 제한사항
- genres[i]는 고유번호가 i인 노래의 장르입니다.
- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
- 장르 종류는 100개 미만입니다.
- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
- 모든 장르는 재생된 횟수가 다릅니다.

<details>
<summary>입출력 예 보이기 / 숨기기</summary>
<div markdown="1">

##### 입출력 예

| clothes                                         | plays                      | return       |
| ----------------------------------------------- | -------------------------- | ------------ |
| ["classic", "pop", "classic", "classic", "pop"] | [500, 600, 150, 800, 2500] | [4, 1, 3, 0] |

###### 입출력 예 설명
classic 장르는 1,450회 재생되었으며, classic 노래는 다음과 같습니다.
- 고유 번호 3: 800회 재생
- 고유 번호 0: 500회 재생
- 고유 번호 2: 150회 재생

pop 장르는 3,100회 재생되었으며, pop 노래는 다음과 같습니다.
- 고유 번호 4: 2,500회 재생
- 고유 번호 1: 600회 재생

따라서 pop 장르의 [4, 1]번 노래를 먼저, classic 장르의 [3, 0]번 노래를 그다음에 수록합니다.
</div>
</details><br>

##### 문제 풀이
앨범들을 한차례 정렬한 후 가장 위의 두 곡, 혹은 한 곡을 뽑는다. 시간 복잡도는 정렬로 인한 $O(n\log{n})$으로 예상된다.

[`JavaScript`](#베스트앨범-javascript)

<h5 id="베스트앨범-javascript">JavaScript</h5>

```js
function solution(genres, plays) {
  const answer = [];
  const musicMap = new Map();
  const countMap = new Map();
  for (let i = 0; i < genres.length; i++) {
      const mapGenre = musicMap.get(genres[i]);
      if (mapGenre) {
          mapGenre.set(i, plays[i]);
          countMap.set(genres[i], countMap.get(genres[i]) + plays[i]);
      }
      else {
          musicMap.set(genres[i], new Map().set(i, plays[i]));
          countMap.set(genres[i], plays[i]);
      }
  }
  // sort genres by total plays
  const count = Array.from(countMap.entries());
  count.sort(([genre1, play1], [genre2, play2]) => {
      return +(play1 < play2) || +(play1 === play2) - 1;        
  });
  
  for (let i = 0; i < count.length; i++) {
      const map = musicMap.get(count[i][0]);
      const maxUpto2 = [];
      map.forEach((value, key) => {
          // if equal, existing one has a lower unique number.
          if (maxUpto2.length === 2) {
              if (maxUpto2[0].value < value) {
                  maxUpto2.unshift({ key, value });
                  maxUpto2.pop();
              } else if (maxUpto2[1].value < value) {
                  maxUpto2[1] = { key, value };
              }
          } else if (maxUpto2.length === 1) {
              if (maxUpto2[0].value < value) {
                  maxUpto2.unshift({ key, value });
              } else {
                  maxUpto2.push({ key, value });
              } 
          } else {
              maxUpto2.push({ key, value });
          }
      });
      for (let j = 0; j < maxUpto2.length; j++) answer.push(maxUpto2[j].key);
  }
  
  return answer;
}
```

---

##### 문제 출처  
프로그래머스 코딩 테스트 연습: <https://programmers.co.kr/learn/challenges>

<style>
  table {
    table-layout: auto;
    width: fit-content;
  }
  table th, table td {
    padding: 8px 12px;
  }
</style>