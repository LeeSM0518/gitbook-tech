---
cover: ../.gitbook/assets/Frame 81.png
coverY: 0
---

# Stack

## 개념

먼저 들어간 것이 마지막에 나오는 선입후출(FILO, First In Last Out) 방식



## 문제 1. 괄호 회전하기

### 설명

다음 규칙을 지키는 문자열을 올바른 괄호 문자열이라고 정의한다.

* `()` , `[]` , `{}` 는 모두 올바른 괄호 문자열이다.
* 만약 A가 올바른 괄호 문자열이라면, `(A)`, `[A]`, `{A}` 도 올바른 괄호 문자열이다.
* 만약 A, B가 올바른 괄호 문자열이라면, AB도 올바른 괄호 문자열이다.
  * A = `{}` , B = `([])`&#x20;
  * `{}([])` 도 올바른 괄호 문자열
* 대괄호, 중괄호 그리고 소괄호로 이루어진 문자열 s가 매개변수로 주어진다.
* s를 왼쪽으로 x (0 <= x < s의 길이) 칸 만큼 회전시켰을 때 s가 올바른 괄호 문자열이 되게 하는 x의 개수를 반환하는 solution() 함수를 완성해라.

### 제약 조건

* s의 길이는 1 이상 1,000 이하

### 입출력의 예

| s        | result |
| -------- | ------ |
| `[](){}` | 3      |
| `}]()[{` | 2      |

### 코드

```kotlin
import java.util.Stack

class Solution {
    fun isValid(s: String): Boolean {
        val stack = Stack<Char>()

        for (char in s) {
            // 2.1. 현재 문자를 확인
            when (char) {
                '(', '{', '[' -> stack.push(char)
                ')', '}', ']' -> {
                    // 스택에서 하나를 peek 함
                    if (stack.isEmpty()) return false
                    val top = stack.peek()
                    // 2.2. 현재 괄호와 peek한 괄호가 올바른 괄호가 동일한지 확인
                    if ((char == ')' && top == '(') ||
                        (char == '}' && top == '{') ||
                        (char == ']' && top == '[')) {
                        // 2.2.1. 동일할 경우 pop 한 후 횟수 증가
                        stack.pop()
                    } else {
                        // 2.2.2. 동일하지 않을 경우 false 반환
                        return false
                    }
                }
            }
        }
        return stack.isEmpty()
    }
    
    fun solution(s: String): Int {
        var count = 0
        val length = s.length

        for (i in 0 until length) {
            // 1. 문자열을 왼쪽으로 이동시킴
            val rotated = s.substring(i) + s.substring(0, i)

            // 2. 문자열을 순회하여 올바른 괄호인지 확인
            if (isValid(rotated)) {
                count++
            }
        }
        return count
    }
}
```



## 참고

{% embed url="https://school.programmers.co.kr/learn/courses/30/lessons/76502?language=kotlin" %}
괄호 회전하기
{% endembed %}
