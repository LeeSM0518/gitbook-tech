---
cover: ../.gitbook/assets/Frame 81.png
coverY: 0
---

# Hash

## 개념

해시 함수를 사용해서 변환한 값을 인덱스로 삼아 키와 값을 저장해서 빠른 데이터 탐색을 제공하는 자료구조이다.



## 문제 1. 완주하지 못한 선수

### 설명

많은 선수 중 단 한 명의 선수를 제외하고 모든 선수가 마라톤을 완주하였습니다. 마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 있을 때 완주하지 못한 선수의 이름을 반환하는 solution() 함수를 작성하세요.

### 제약 조건

* 마라톤 경기에 참여한 선수 수는 1명 이상 100,000명 이하입니다.
* completion 길이는 participant 길이보다 1 작습니다.
* 참가자 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
* 참가자 중에는 동명이인이 있을 수 있습니다.

### 입출력의 예

| participant                                         | completion                                 | return   |
| --------------------------------------------------- | ------------------------------------------ | -------- |
| \["leo", "kiki", "eden"]                            | \["eden", "kiki"]                          | "leo"    |
| \["marina", "josipa", "nikola", "vinko", "filipa" ] | \["josipa", "filipa", "marina", "nikola" ] | "vinko"  |
| \["mislav", "stanko", "mislav", "ana"]              | \["stanko", "ana", "mislav"]               | "mislav" |

### 코드

```kotlin
fun solution(participant: List<String>, completion: List<String>): String {
    var result = ""

    // 1. key는 String이고 value는 Int인 hashmap을 정의
    val hashmap = HashMap<String, Int>()

    // 2. participant 순회
    for (name in participant) {
        // 2.1. participant 값이 hashmap의 key로 존재하는지 확인
        if (hashmap.containsKey(name)) {
            // 2.1.true. 해당 key의 value를 1 올림
            hashmap[name] = hashmap[name]!! + 1
        } else {
            // 2.1.false. 해당 key를 추가하고 value를 1 올림
            hashmap[name] = 1
        }
    }

    // 3. completion 순회
    for (name in completion) {
        // 3.1. completion 값이 hashmap의 key로 존재하는지 확인
        if (hashmap.containsKey(name)) {
            // 3.1.true.1. 존재할 경우 해당 key의 value를 1 낮춤
            hashmap[name] = hashmap[name]!! - 1
            // 3.1.true.2. value가 0일 경우 해당 key를 hashmap에서 제거
            if (hashmap[name] == 0) {
                hashmap.remove(name)
            }
        }
    }

    // 4. hashmap의 첫 번째 key를 result로 저장
    result = hashmap.keys.first()

    return result
}
```

