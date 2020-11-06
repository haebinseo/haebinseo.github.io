---
layout: post
title: "스택/큐 문제 풀이"
date: "2020-11-06 23:10:00"
author: Haebin Seo
categories: coding-test
tags: coding-test stack queue
use_math: true
---
문제 바로가기  
[`기능개발`](#기능개발) [`다리를 지나는 트럭`](#다리를-지나는-트럭) [`프린터`](#프린터)

### [기능개발](https://programmers.co.kr/learn/courses/30/lessons/42586?language=javascript)
`level 2`
##### 문제 설명
프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

##### 제한사항
- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

<details>
<summary>입출력 예 보이기 / 숨기기</summary>
<div markdown="1">
##### 입출력 예

| progresses               | speeds     | return    |
| ------------------------ | ---------- | --------- |
| [93, 30, 55]             | [1, 30, 5] | [2, 1]    |
| [95, 90, 99, 99, 80, 99] | [1, 30, 5] | [1, 3, 2] |

###### 입출력 예 설명
입출력 예 #1
첫 번째 기능은 93% 완료되어 있고 하루에 1%씩 작업이 가능하므로 7일간 작업 후 배포가 가능합니다.
두 번째 기능은 30%가 완료되어 있고 하루에 30%씩 작업이 가능하므로 3일간 작업 후 배포가 가능합니다. 하지만 이전 첫 번째 기능이 아직 완성된 상태가 아니기 때문에 첫 번째 기능이 배포되는 7일째 배포됩니다.
세 번째 기능은 55%가 완료되어 있고 하루에 5%씩 작업이 가능하므로 9일간 작업 후 배포가 가능합니다.

따라서 7일째에 2개의 기능, 9일째에 1개의 기능이 배포됩니다.

입출력 예 #2
모든 기능이 하루에 1%씩 작업이 가능하므로, 작업이 끝나기까지 남은 일수는 각각 5일, 10일, 1일, 1일, 20일, 1일입니다. 어떤 기능이 먼저 완성되었더라도 앞에 있는 모든 기능이 완성되지 않으면 배포가 불가능합니다.

따라서 5일째에 1개의 기능, 10일째에 3개의 기능, 20일째에 2개의 기능이 배포됩니다.
</div>
</details><br>

##### 문제 풀이
[`JavaScript`](#기능개발-javascript)

<h5 id="기능개발-javascript">JavaScript</h5>

```js
function solution(progresses, speeds) {
  // 각 기능의 배포 일
  const releaseDays = progresses.map((progress, idx) => {
    const progressLeft = 100 - progress;
    return Math.ceil(progressLeft / speeds[idx]);
  });
  // 배포일과 배포되는 기능의 개수 정리
  let len;
  const releases = releaseDays.reduce((acc, day) => {
    len = acc.length;
    if (!len) acc.push({ release: day, count: 1 });
    else if (acc[len - 1].release < day) acc.push({ release: day, count: 1 });
    else acc[len - 1].count += 1;

    return acc;
  }, []);
  return releases.map(r => r.count);
}
```

---

### [다리를 지나는 트럭](https://programmers.co.kr/learn/courses/30/lessons/42583?language=javascript)
`level 2`
##### 문제 설명
트럭 여러 대가 강을 가로지르는 일 차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 트럭은 1초에 1만큼 움직이며, 다리 길이는 bridge_length이고 다리는 무게 weight까지 견딥니다.
※ 트럭이 다리에 완전히 오르지 않은 경우, 이 트럭의 무게는 고려하지 않습니다.

예를 들어, 길이가 2이고 10kg 무게를 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.

| 경과 시간 | 다리를 지난 트럭 | 다리를 건너는 트럭 | 대기 트럭 |
| --------- | ---------------- | ------------------ | --------- |
| 0         | []               | []                 | [7,4,5,6] |
| 1~2       | []               | [7]                | [4,5,6]   |
| 3         | [7]              | [4]                | [5,6]     |
| 4         | [7]              | [4,5]              | [6]       |
| 5         | [7,4]            | [5]                | [6]       |
| 6~7       | [7,4,5]          | [6]                | []        |
| 8         | [7,4,5,6]        | []                 | []        |

따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.

solution 함수의 매개변수로 다리 길이 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

##### 제한사항
- bridge_length는 1 이상 10,000 이하입니다.
- weight는 1 이상 10,000 이하입니다.
- truck_weights의 길이는 1 이상 10,000 이하입니다.
- 모든 트럭의 무게는 1 이상 weight 이하입니다.

<details>
<summary>입출력 예 보이기 / 숨기기</summary>
<div markdown="1">

##### 입출력 예

| bridge_length | weight | truck_weights                   | return |
| ------------- | ------ | ------------------------------- | ------ |
| 2             | 10     | [7,4,5,6]                       | 8      |
| 100           | 100    | [10]                            | 101    |
| 100           | 100    | [10,10,10,10,10,10,10,10,10,10] | 110    |

</div>
</details><br>

##### 문제 풀이
[`JavaScript`](#다리를-지나는-트럭-javascript)

<h5 id="다리를-지나는-트럭-javascript">JavaScript</h5>

```js
function solution(bridge_length, weight, truck_weights) {
  let cTime = 1; // current time
  let weightAcc  = 0; // 누적 중량
  const truckExitTime = []; // 트럭이 다리를 벗어날 것으로 예상되는 시간의 queue
  let queueHeadIdx = 0; // truckExitTime에서 최근에 나간 트럭의 index
  for(let i = 0; i < truck_weights.length; i++) {
    while (weightAcc + truck_weights[i] > weight) {
      // 진입 대기 중인 트럭을 포함한 다리 위 모든 트럭 중량 합이 한도를 초과할 때
      cTime = truckExitTime[queueHeadIdx];
      weightAcc -= truck_weights[queueHeadIdx];
      queueHeadIdx++;
    }
    while (cTime >= truckExitTime[queueHeadIdx]) {
      // 다리를 지난 트럭이 존재할 때
      weightAcc -= truck_weights[queueHeadIdx];
      queueHeadIdx++;
    }
    // 대기 중인 트럭 다리 진입
    weightAcc += truck_weights[i];
    truckExitTime.push(cTime + bridge_length);
    cTime++;
  }
  return truckExitTime[truckExitTime.length - 1];
}
```

---

### [프린터](https://programmers.co.kr/learn/courses/30/lessons/42587?language=javascript)
`level 2`
##### 문제 설명
일반적인 프린터는 인쇄 요청이 들어온 순서대로 인쇄합니다. 그렇기 때문에 중요한 문서가 나중에 인쇄될 수 있습니다. 이런 문제를 보완하기 위해 중요도가 높은 문서를 먼저 인쇄하는 프린터를 개발했습니다. 이 새롭게 개발한 프린터는 아래와 같은 방식으로 인쇄 작업을 수행합니다.

```txt
1. 인쇄 대기목록의 가장 앞에 있는 문서(J)를 대기목록에서 꺼냅니다.
2. 나머지 인쇄 대기목록에서 J보다 중요도가 높은 문서가 한 개라도 존재하면 J를 대기목록의 가장 마지막에 넣습니다.
3. 그렇지 않으면 J를 인쇄합니다.
```

예를 들어, 4개의 문서(A, B, C, D)가 순서대로 인쇄 대기목록에 있고 중요도가 2 1 3 2 라면 C D A B 순으로 인쇄하게 됩니다.

내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 알고 싶습니다. 위의 예에서 C는 1번째로, A는 3번째로 인쇄됩니다.

현재 대기목록에 있는 문서의 중요도가 순서대로 담긴 배열 priorities와 내가 인쇄를 요청한 문서가 현재 대기목록의 어떤 위치에 있는지를 알려주는 location이 매개변수로 주어질 때, 내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 return 하도록 solution 함수를 작성해주세요.

##### 제한사항
- 현재 대기목록에는 1개 이상 100개 이하의 문서가 있습니다.
- 인쇄 작업의 중요도는 1~9로 표현하며 숫자가 클수록 중요하다는 뜻입니다.
- location은 0 이상 (현재 대기목록에 있는 작업 수 - 1) 이하의 값을 가지며 대기목록의 가장 앞에 있으면 0, 두 번째에 있으면 1로 표현합니다.

<details>
<summary>입출력 예 보이기 / 숨기기</summary>
<div markdown="1">

##### 입출력 예

| priorities         | location | return |
| ------------------ | -------- | ------ |
| [2, 1, 3, 2]       | 2        | 1      |
| [1, 1, 9, 1, 1, 1] | 0        | 5      |

##### 입출력 예 설명
예제 #1
문제에 나온 예와 같습니다.

예제 #2
6개의 문서(A, B, C, D, E, F)가 인쇄 대기목록에 있고 중요도가 1 1 9 1 1 1 이므로 C D E F A B 순으로 인쇄합니다.
</div>
</details><br>

##### 문제 풀이
[`JavaScript`](#프린터-javascript)

<h5 id="프린터-javascript">JavaScript</h5>

```js
// queue
function solution(priorities, location) {
  const list = priorities.map((p, idx) => {
    return {
      isTarget: idx === location,
      priority: p,
    };
  });
  let count = 0;

  while(true) {
    const cur = list.shift();
    if (list.some(elem => cur.priority < elem.priority)) {
      list.push(cur);
    } else {
      count++;
      if(cur.isTarget) return count;
    }
  }
}

// using hash
function solution(priorities, location) {
  let printOrder = 1;
  const priorityHash = new Map(); // key: priority, value: [ locations ]
  const targetPriority = priorities[location];
  let currentLoc = 0;

  for (let i = 0; i < 10; i++) priorityHash.set(i, []);
  // priority hash table 작성
  for (let idx = 0; idx < priorities.length; idx++) {
    priorityHash.get(priorities[idx]).push(idx);
  }
  // 타겟 문서의 우선순위보다 높은 문서들의 출력 순서 추적
  for (let i = 9; i > targetPriority; i--) {
    const arr = priorityHash.get(i);
    if (!arr.length) continue;
    // 현 우선순위에서 가장 마지막으로 출력되는 문서의 해당 배열에서의 위치, 대기목록에서의 위치가 x.
    const idxInArr = arr.findIndex((loc) => loc >= currentLoc) - 1;
    // 현재 출력 위치를 업데이트
    currentLoc = idxInArr > -1 ? arr[idxInArr] : arr[arr.length - 1];
    printOrder += arr.length;
  }
  // 최근 출력한 위치에서 타겟 문서까지 추가로 출력해야 될 문서 확인
  const targetArr = priorityHash.get(targetPriority);
  const idxInArr = targetArr.findIndex((loc) => loc >= currentLoc);
  const idxOfTarget = targetArr.indexOf(location);
  printOrder += idxOfTarget - (idxInArr > -1 ? idxInArr : 0);
  printOrder += idxInArr > idxOfTarget ? targetArr.length : 0;
  
  return printOrder;
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