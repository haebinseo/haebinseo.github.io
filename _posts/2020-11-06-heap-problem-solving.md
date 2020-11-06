---
layout: post
title: "힙 문제 풀이"
date: "2020-11-06 23:58:00"
author: Haebin Seo
categories: coding-test
tags: coding-test heap priority-queue
---
문제 바로가기  
[`디스크 컨트롤러`](#디스크-컨트롤러) [`이중우선순위큐`](#이중우선순위큐)

### [디스크 컨트롤러](https://programmers.co.kr/learn/courses/30/lessons/42627?language=javascript)
`level 3`
##### 문제 설명
하드디스크는 한 번에 하나의 작업만 수행할 수 있습니다. 디스크 컨트롤러를 구현하는 방법은 여러 가지가 있습니다. 가장 일반적인 방법은 요청이 들어온 순서대로 처리하는 것입니다.

예를들어
```txt
0ms 시점에 3ms가 소요되는 A작업 요청
1ms 시점에 9ms가 소요되는 B작업 요청
2ms 시점에 6ms가 소요되는 C작업 요청
```
와 같은 요청이 들어왔습니다. 이를 그림으로 표현하면 아래와 같습니다.
![](https://grepp-programmers.s3.amazonaws.com/files/production/b68eb5cec6/38dc6a53-2d21-4c72-90ac-f059729c51d5.png)
한 번에 하나의 요청만을 수행할 수 있기 때문에 각각의 작업을 요청받은 순서대로 처리하면 다음과 같이 처리 됩니다.
![](https://grepp-programmers.s3.amazonaws.com/files/production/5e677b4646/90b91fde-cac4-42c1-98b8-8f8431c52dcf.png)
```txt
A: 3ms 시점에 작업 완료 (요청에서 종료까지 : 3ms)
B: 1ms부터 대기하다가, 3ms 시점에 작업을 시작해서 12ms 시점에 작업 완료(요청에서 종료까지 : 11ms)
C: 2ms부터 대기하다가, 12ms 시점에 작업을 시작해서 18ms 시점에 작업 완료(요청에서 종료까지 : 16ms)
```
이 때 각 작업의 요청부터 종료까지 걸린 시간의 평균은 10ms(= (3 + 11 + 16) / 3)가 됩니다.

하지만 A → C → B 순서대로 처리하면
![](https://grepp-programmers.s3.amazonaws.com/files/production/9eb7c5a6f1/a6cff04d-86bb-4b5b-98bf-6359158940ac.png)
```txt
A: 3ms 시점에 작업 완료(요청에서 종료까지 : 3ms)
C: 2ms부터 대기하다가, 3ms 시점에 작업을 시작해서 9ms 시점에 작업 완료(요청에서 종료까지 : 7ms)
B: 1ms부터 대기하다가, 9ms 시점에 작업을 시작해서 18ms 시점에 작업 완료(요청에서 종료까지 : 17ms)
```
이렇게 A → C → B의 순서로 처리하면 각 작업의 요청부터 종료까지 걸린 시간의 평균은 9ms(= (3 + 7 + 17) / 3)가 됩니다.

각 작업에 대해 [작업이 요청되는 시점, 작업의 소요시간]을 담은 2차원 배열 jobs가 매개변수로 주어질 때, 작업의 요청부터 종료까지 걸린 시간의 평균을 가장 줄이는 방법으로 처리하면 평균이 얼마가 되는지 return 하도록 solution 함수를 작성해주세요. (단, 소수점 이하의 수는 버립니다)

##### 제한사항
- jobs의 길이는 1 이상 500 이하입니다.
- jobs의 각 행은 하나의 작업에 대한 [작업이 요청되는 시점, 작업의 소요시간] 입니다.
- 각 작업에 대해 작업이 요청되는 시간은 0 이상 1,000 이하입니다.
- 각 작업에 대해 작업의 소요시간은 1 이상 1,000 이하입니다.
- 하드디스크가 작업을 수행하고 있지 않을 때에는 먼저 요청이 들어온 작업부터 처리합니다.

<details>
<summary>입출력 예 보이기 / 숨기기</summary>
<div markdown="1">
##### 입출력 예

| jobs                      | return |
| ------------------------- | ------ |
| \[[0, 3], [1, 9], [2, 6]] | 9      |

###### 입출력 예 설명
문제에 주어진 예와 같습니다.

0ms 시점에 3ms 걸리는 작업 요청이 들어옵니다.
1ms 시점에 9ms 걸리는 작업 요청이 들어옵니다.
2ms 시점에 6ms 걸리는 작업 요청이 들어옵니다.
</div>
</details><br>

##### 문제 풀이
[`JavaScript`](#디스크-컨트롤러-javascript)

<h5 id="디스크-컨트롤러-javascript">JavaScript</h5>

```js
class PriorityQueue {
  constructor() {
    this.store = [-1];
  }

  arrange(fromBottom) {
    if (fromBottom) {
      let idx = this.store.length - 1;
      while(idx > 1) {
        const parentIdx = Math.floor(idx/2);
        if (this.store[idx].priority >= this.store[parentIdx].priority) break;
        
        [this.store[idx], this.store[parentIdx]] = [this.store[parentIdx], this.store[idx]];
        idx = parentIdx;
      }
    } else {
      const len = this.store.length;
      let idx = 1;
      let leftChildIdx = idx * 2;
      let rightChildIdx = leftChildIdx + 1;
      while(leftChildIdx < len) {
        // 두 자식중 더 작은 자식의 인덱스
        let minChildIdx = leftChildIdx;
        if (rightChildIdx < len && this.store[leftChildIdx].priority > this.store[rightChildIdx].priority) {
          minChildIdx = rightChildIdx;
        }

        if (this.store[idx].priority > this.store[minChildIdx].priority) {
          [this.store[idx], this.store[minChildIdx]] = [this.store[minChildIdx], this.store[idx]];
          idx = minChildIdx;
        } else {
          break;
        }
        leftChildIdx = idx * 2;
        rightChildIdx = leftChildIdx + 1; 
      }
    }
  }

  push({ priority, data }) {
    this.store.push({ priority, data });
    this.arrange(true);
  }

  pop() {
    const len = this.store.length;
    if (len === 1) throw new Error('there is nothing to pop');
    else if (len === 2) return this.store.pop();
    
    [this.store[1], this.store[len - 1]] = [this.store[len - 1], this.store[1]];
    const top = this.store.pop();
    this.arrange(false);
    return top;
  }

  isEmpty() {
    return this.store.length === 1;
  }

  print() {
    this.store.forEach((elem, idx) => {
      if (idx === 0) return;
      console.log(`${idx}th elem: { priority: ${elem.priority}, data: ${elem.data} }`);
    })
  }
}

function solution(jobs) {
  let fromReqToFinAcc = 0;
  jobs.sort((a, b) => {
    if (a[0] === b[0]) return a[1] - b[1];
    else return a[0] - b[0];
  });
  let currentTime = 0;
  const pq = new PriorityQueue(); // 우선순위: 소요시간. 작을 수록 높은 우선순위임
  let nextJobIdx = 0;

  // 시간에 따라 priority queue에 작업을 추가하며 수행
  while (nextJobIdx < jobs.length) {
    if (jobs[nextJobIdx][0] <= currentTime) {
      // 시간이 지나면서 요청이 들어온 작업 priority queue에 추가
      pq.push({ priority: jobs[nextJobIdx][1], data: jobs[nextJobIdx][0]});
      nextJobIdx++;
      continue;
    }
    if (pq.isEmpty()) {
      // 현 시간에 요청이 들어온 작업이 없을 경우, 이후 제일 먼저 요청이 들어오는 작업 수행
      pq.push({ priority: jobs[nextJobIdx][1], data: jobs[nextJobIdx][0]});
      currentTime = jobs[nextJobIdx][0];
      nextJobIdx++;
      continue;
    }
    // priority queue에서 가장 우선순위가 높은 작업 수행
    const top = pq.pop();
    fromReqToFinAcc += top.priority + currentTime - top.data;
    currentTime += top.priority;
  }
  // priority queue에 남아있는 작업을 수행
  while (!pq.isEmpty()) {
    const top = pq.pop();
    fromReqToFinAcc += top.priority + currentTime - top.data;
    currentTime += top.priority;
  }

  return Math.floor(fromReqToFinAcc / jobs.length);
}
```

---

### [이중우선순위큐](https://programmers.co.kr/learn/courses/30/lessons/42628?language=javascript)
`level 3`
##### 문제 설명
이중 우선순위 큐는 다음 연산을 할 수 있는 자료구조를 말합니다.

| 명령어 | 수신 탑(높이)                  |
| ------ | ------------------------------ |
| I 숫자 | 큐에 주어진 숫자를 삽입합니다. |
| D 1    | 큐에서 최댓값을 삭제합니다.    |
| D -1   | 큐에서 최솟값을 삭제합니다.    |

이중 우선순위 큐가 할 연산 operations가 매개변수로 주어질 때, 모든 연산을 처리한 후 큐가 비어있으면 [0,0] 비어있지 않으면 [최댓값, 최솟값]을 return 하도록 solution 함수를 구현해주세요.

##### 제한사항
- operations는 길이가 1 이상 1,000,000 이하인 문자열 배열입니다.
- operations의 원소는 큐가 수행할 연산을 나타냅니다.
- 원소는 “명령어 데이터” 형식으로 주어집니다.
- 최댓값/최솟값을 삭제하는 연산에서 최댓값/최솟값이 둘 이상인 경우, 하나만 삭제합니다.
- 빈 큐에 데이터를 삭제하라는 연산이 주어질 경우, 해당 연산은 무시합니다.

<details>
<summary>입출력 예 보이기 / 숨기기</summary>
<div markdown="1">
##### 입출력 예

| operations                     | return |
| ------------------------------ | ------ |
| ["I 16", "D 1"]                | [0,0]  |
| ["I 7", "I 5", "I -5", "D -1"] | [7,5]  |

###### 입출력 예 설명
16을 삽입 후 최댓값을 삭제합니다. 비어있으므로 [0,0]을 반환합니다.
7,5,-5를 삽입 후 최솟값을 삭제합니다. 최대값 7, 최소값 5를 반환합니다.
</div>
</details><br>

##### 문제 풀이
우선순위큐를 이용해 풀기는 했는데 그냥 쌩 max, min 찾는게 더 빠르네...?
[`JavaScript`](#이중우선순위큐-javascript)

<h5 id="이중우선순위큐-javascript">JavaScript</h5>

```js
// 그냥 쌩으로
function solution(arg) {
    var list = [];
    arg.forEach(t=>{
        if(t[0] === 'I'){
            list.push(+(t.split(" ")[1]));            
        }
        else{
            if(!list.length) return;

            var val = (t[2] === '-' ? Math.min : Math.max)(...list);
            var delIndex = list.findIndex(t=> t ===  val);

            list.splice(delIndex, 1);
        }
    })

    return list.length ? [Math.max(...list), Math.min(...list)] : [0, 0];
}

// Priority Queue
class PriorityQueue {
  constructor(order) {
    this.store = [NaN];
    this.order = order;
  }

  arrange(fromBottom, removedIdx = 1) {
    if (fromBottom) {
      let idx = this.store.length - 1;
      while(idx > 1) {
        const parentIdx = Math.floor(idx/2);
        if (this.order === "asc") {
          if (this.store[idx] >= this.store[parentIdx]) break;
        } else {
          if (this.store[idx] <= this.store[parentIdx]) break;
        }
                
        [this.store[idx], this.store[parentIdx]] = [this.store[parentIdx], this.store[idx]];
        idx = parentIdx;
      }
    } else {
      const len = this.store.length;
      let idx = removedIdx;
      let leftChildIdx = idx * 2;
      let rightChildIdx = leftChildIdx + 1;
      while(leftChildIdx < len) {
        let minChildIdx = leftChildIdx;
        if (this.order === "asc") {
          // 두 자식중 더 작은 자식의 인덱스
          if (rightChildIdx < len && this.store[leftChildIdx] > this.store[rightChildIdx]) {
            minChildIdx = rightChildIdx;
          }

          if (this.store[idx] < this.store[minChildIdx]) break; 
          
          [this.store[idx], this.store[minChildIdx]] = [this.store[minChildIdx], this.store[idx]];
          idx = minChildIdx;
        } else {
          // 두 자식중 더 큰 자식의 인덱스
          if (rightChildIdx < len && this.store[leftChildIdx] < this.store[rightChildIdx]) {
            minChildIdx = rightChildIdx;
          }

          if (this.store[idx] > this.store[minChildIdx]) break;

          [this.store[idx], this.store[minChildIdx]] = [this.store[minChildIdx], this.store[idx]];
          idx = minChildIdx;
        }
        leftChildIdx = idx * 2;
        rightChildIdx = leftChildIdx + 1; 
      }
    }
  }

  push(priority) {
    this.store.push(priority);
    this.arrange(true);
  }

  pop() {
    const len = this.store.length;
    if (len === 1) throw new Error('there is nothing to pop');
    else if (len === 2) return this.store.pop();
    
    [this.store[1], this.store[len - 1]] = [this.store[len - 1], this.store[1]];
    const top = this.store.pop();
    this.arrange(false);
    return top;
  }

  isEmpty() {
    return this.store.length === 1;
  }

  print() {
    this.store.forEach((elem, idx) => {
      console.log(`${idx}th elem: ${elem}`);
    })
  }
}

function solution(operations) {
  const minPQ = new PriorityQueue('asc');
  const maxPQ = new PriorityQueue('desc');
  

  for (let i = 0; i < operations.length; i++) {
    if (operations[i].charAt(0) === 'I') {
      // insert
      const num = parseInt(operations[i].replace(/[^0-9-]/g,""), 10);
      minPQ.push(num);
      maxPQ.push(num);
    } else {
      // delete
      const [targetPQ, otherPQ] = operations[i].charAt(2) === '-' ? [minPQ, maxPQ] : [maxPQ, minPQ];
      if (targetPQ.isEmpty()) continue;

      const len = targetPQ.store.length;
      if (len === 2) {
        targetPQ.pop();
        otherPQ.pop();
      } else {
        const min = targetPQ.pop();
        const idxToDelete = otherPQ.store.indexOf(min);
        [otherPQ.store[len - 1], otherPQ.store[idxToDelete]] = [otherPQ.store[idxToDelete], otherPQ.store[len - 1]];
        otherPQ.store.pop();
        otherPQ.arrange(false, idxToDelete);
      }
      
    }
  }

  return minPQ.isEmpty() ? [0, 0] : [maxPQ.pop(), minPQ.pop()];
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