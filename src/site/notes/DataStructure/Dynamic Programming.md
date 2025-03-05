---
{"dg-publish":true,"permalink":"/data-structure/dynamic-programming/"}
---

한줄 요약: 작은 부분문제들이 반복되는 것을 이용해 문제를 푼다. 정답을 구하고 그 답을 메모해두고 그 다음 문제를 풀고 앞의 답을 이용한다. 수열에서 점화식을 구하는거랑 비슷한 느낌이다.

#### 사용 조건
1. 작은 문제의 반복
2. 같은 문제는 구할 때 마다 같은 값

#### Memoization
작은 문제 계산 값을 저장해놓고 다시 사용하는 것
```python
def fib(n):
	if n < 0:
		raise IndexError(
			'Index was negative.'
			'No such thing as a negative index in a series.'
			)
	elif n in [0,1]:
		return n
	print("computing fib(%i)" % n)
	return fib(n-1) + fib(n-2)
```
n번째 피보나치 수를 계산하는 재귀식. 같은 입력값에 대해 여러번 실행함
덧셈 연산이 2**(n-2)-1번 이루어지고 시간복잡도는 O(2^n)
```python
def me(n):
	memo[0] = 1
	memo[1] = 1
	if n<2:
		return memo[n]
	for i in range(2,n+1):
		memo[i] = memo[i-2] + memo[i-1]
	return memo[n]
if __name__ = '__main__':
	n = int(input())
	me = [0 for i in range(n+2)]
	print(me(n))
	print(me)
```

#### 타일링 문제풀이
