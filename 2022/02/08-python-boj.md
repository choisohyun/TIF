# 오늘 공부(22/02/08)

- 파이썬 4문제 풀이
- 파이썬은 회사 프로젝트 배포관련 스크립트에서도 쓰여서 문법을 익혀 두면 나쁠 것이 없다고 생각해서 간단히 몇 문제 풀어 보았다.

# 알게 된 것

- 파이썬은 삼항연산자도 말하는 것처럼 사용된다. 
- 파이썬: `1 if y > 0 else 4`
- 자바스크립트: `y > 0 ? 1 : 4`
- 형변환도 함수 호출하듯이 int(), list() 형식이다. 

### https://www.acmicpc.net/problem/10926

```py
id = input()
print(id + '??!')
```

### https://www.acmicpc.net/problem/18108

```py
year = int(input())
print(year - 543)
```

### https://www.acmicpc.net/problem/14681

```py
x = int(input())
y = int(input())

if (x > 0):
    print(1 if y > 0 else 4)
else:
    print(2 if y > 0 else 3)
```
    
### https://www.acmicpc.net/problem/2884

```py
import math

hour, min = list(map(int, input().split()))

hour = 24 if hour == 0 and min < 45 else hour
early_time = hour * 60 + min - 45
early_hour = math.floor(early_time / 60)
early_min = early_time % 60

print(early_hour, early_min)    
```
