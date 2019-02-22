## Hash Questions

---

> #1 완주하지 못한 선수


마라톤에 참여한 선수들의 이름이 담긴 배열 participants와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

**제한사항**
- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다
- completion의 길이는 participant의 길이보다 1 작습니다
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다
- 참가자 중에는 동명이인이 있을 수 있습니다


> 풀이

```
import java.util.*;

class Solution {
    public String solution(String[] participant, String[] completion) {

        // 두 배열의 길이가 1 차이나므로 우선 각각의 배열을 정리한다.
        Arrays.sort(participant);
        Arrays.sort(completion);
        
        int i;
        for (i = 0; i < completion.length; i++) {
            // 두 배열을 비교하여 완주하지 못한 선수의 이름을 반환한다
            if (!participant[i].equals(completion[i])) {
                return participant[i];
            }
        }

        return participant[i];
    }
}
```

---

문제출처

[#1. 완주하지 못한 선수](https://programmers.co.kr/learn/courses/30/lessons/42576)
