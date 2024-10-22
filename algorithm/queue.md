---
cover: ../.gitbook/assets/Frame 81.png
coverY: 0
---

# Queue

## 개념

먼저 들어간 것이 먼저 나오는 선입선출(FIFO, First In Fast Out) 방식



## 문제 1. 기능 개발

### 설명

프로그래머스팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다. 또 각 기능의 개발 속도는 모두 다르므로 뒤의 기능이 앞의 기능보다 먼저 개발될 수도 있습니다. 이때, 뒤의 기능은 앞의 기능이 배포될 때 함께 배포되어야 합니다.

배포 순서대로 작업 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 반환하도록 solution() 함수를 완성하세요.

### 제약 조건

* 작업 개수(progresses, speeds의 배열 길이)는 100개 이하입니다.
* 작업 진도는 100 미만의 자연수입니다.
* 작업 속도는 100 이하의 자연수입니다.
* 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다.
  * 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

### 입출력의 예

| progresses                | speeds           | return     |
| ------------------------- | ---------------- | ---------- |
| \[93, 30, 55]             | \[1, 30, 5]      | \[2, 1]    |
| \[95, 90, 99, 99, 80, 99] | \[1, 1, 1, 1, 1] | \[1, 3, 2] |

### 코드

```kotlin
fun solution(progresses: IntArray, speeds: IntArray): IntArray {
    val result = mutableListOf<Int>()

    // 1. progresses와 speeds를 각 인덱스의 값을 key와 value로 만든 객체를 Queue에 입력함
    val queue: Queue<Pair<Int, Int>> = LinkedList()
    for (i in progresses.indices) {
        queue.offer(Pair(progresses[i], speeds[i]))
    }

    // 2. Queue가 비어질 때까지 무한 반복함
    while (queue.isNotEmpty()) {
        // 2.1. Queue에서 첫 번째 원소의 key가 100이 넘을 때까지 계속 순회함
        while (queue.peek().first < 100) {
            // 2.1.1. 각 원소의 key에 value를 더함
            val size = queue.size
            repeat(size) {
                val (progress, speed) = queue.poll()
                queue.offer(Pair(progress + speed, speed))
            }
        }

        // 2.2. Queue에서 key가 100이 넘지 않은 원소까지 추출하면서 개수를 셈
        var count = 0
        while (queue.isNotEmpty() && queue.peek().first >= 100) {
            queue.poll()
            count++
        }

        // 2.3. 개수를 결과로 추가함
        result.add(count)
    }

    return result.toIntArray()
}
```

