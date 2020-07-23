# [파이썬] 함수 파라미터로 리스트 사용시 주의할 점

## 개요

> ```
> 입력
> 0 3 5 4 6 9 2 7 8
> 7 8 2 1 0 5 6 0 9
> 0 6 0 2 7 8 1 3 5
> 3 2 1 0 4 6 8 9 7
> 8 0 4 9 1 3 5 0 6
> 5 9 6 8 2 0 4 1 3
> 9 1 7 6 5 2 0 8 0
> 6 0 3 7 0 1 9 5 2
> 2 5 8 3 9 4 7 6 0
> ```

> > [출처 : 백준알고리즘 2580번](https://www.acmicpc.net/problem/2580)

위와 같은 입력을 한줄씩 리스트로 묶어서 2차원 리스트에 넣는 함수를 만들려고 한다. 함수는 리스트를 파라미터로 받고 입력 값들을 넣어준다. 이 때 값을 넣는 방법에는 크게 두 가지가 있다. 첫 번째는 `append` 를 사용하는 방법이고, 두 번째는 `=` 을 사용하는 방법이다. 그렇다면 이 두 방법은 어떤 차이가 있을까?



## 문제점 

아래의 코드들을 통해 문제점을 생각해보자.

### Way 1 

```python
def get_input1(size, arr_sudoku):
    for i in range(size):
        arr_sudoku.append(list(map(int, input().split())))

size = 9
arr_sudoku = []
get_input1(size, arr_sudoku)
print(arr_sudoku)
```

### 결과

```python
[[0, 3, 5, 4, 6, 9, 2, 7, 8], 
 [7, 8, 2, 1, 0, 5, 6, 0, 9], 
 [0, 6, 0, 2, 7, 8, 1, 3, 5], 
 [3, 2, 1, 0, 4, 6, 8, 9, 7], 
 [8, 0, 4, 9, 1, 3, 5, 0, 6], 
 [5, 9, 6, 8, 2, 0, 4, 1, 3], 
 [9, 1, 7, 6, 5, 2, 0, 8, 0], 
 [6, 0, 3, 7, 0, 1, 9, 5, 2], 
 [2, 5, 8, 3, 9, 4, 7, 6, 0]]
```

### Way 2 

```python
def get_input2(size, arr_sudoku):
    temp = [[0 for i in range(size)] for j in range(size)]
    for i in range(size):
        temp[i] = list(map(int, read_sys().split()))
    arr_sudoku = temp

size = 9
arr_sudoku = []
get_input2(size, arr_sudoku)
print(arr_sudoku)
```

### 결과

```python
[]
```

<strong>두 번째 방법은 리스트에 값이 안들어가는 건가?</strong>



## 원인 

파이썬은 파워풀한 객체지향 언어라서 `a = 4` 같은 코드조차도 a의 주소에 값이 4가 들어가는게 아니라 4라는 값을 지닌 주소를 가리킨다. 

함수에 리스트를 파라미터로 넘기면 리스트의 주소가 넘어간다. 그리고 함수 내에서는 지역변수 개념으로 새로운 주소를 가지는 리스트가 넘어온 리스트의 주소를 가리키는 형식으로 생성된다. 이 때 첫 번째 방법에서 `append` 를 사용했을 때는 참조하는 주소에 값들을 직접 영향을 주면서 바꾸는 반면, 두 번째 방법에서 `=` 을 사용하면 지역변수 리스트는 원래 리스트의 주소를 잃어버리고 새로운 주소를 가리키게 된다. 그렇기 때문에 두 번째 방법에서는 리스트에 아무 것도 들어가지 않은 것이다.



## 해결책

두 번째 방법을 사용하고 싶을 때는 어떡하면 될까? 

정답은  `return` 을 사용하는 것이다.

아래와 같이 코드를 짜면 두 번째 방법도 올바른 결과가 나온다.

```python
def get_input2(size, arr_sudoku):
    temp = [[0 for i in range(size)] for j in range(size)]
    for i in range(size):
        temp[i] = list(map(int, read_sys().split()))
    arr_sudoku = temp
    return arr_sudoku

size = 9
arr_sudoku = []
arr_sudoku = get_input2(size, arr_sudoku)
print(arr_sudoku)
```



 