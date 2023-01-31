# 이것이 코딩테스트다
## 한빛미디어
### 5장 DFS/BFS

pop을 쓰면 마지막 원소 삭제됨

리스트의 마지막 원소부터 출력할려면 list[::-1] 이렇게 쓰기

queue 구현을 위해서는 deque 라이브러리 이용
```python
from collections import deque

queue = deque()
queue.append(5)
queue.append(2)
queue.append(3)
queue.popleft() # 처음 들어온 것을 삭제

queue.reverse() # 역순으로 바꿔서 마지막에 나온 것부터 출력

# 팩토리얼 구현 예제
def factorial_iterative(n):
  result = 1
  for i in range(1, n+1):
    result *= i
  return result

# 팩토리얼을 재귀적으로 구현
def factorial_recursive(n):
  if n<=1:
    return 1
  return n*factorial_recursive(n-1)

```

- DFS : 깊이 우선 탐색, 그래프에서 깊은 부분을 우선적으로 탐색
  - 인접 행렬 : 2차원 배열로 그래프의 연결 관계 표현
  - 인접 리스트 : 리스트로 그래프의 연결 관계 표현

```python
# 인접 행렬 방식
inf = 999999999999999
graph = [
  [0,7,5],
  [7,0,inf],
  [5,inf,0]
]

# 인접 리스트 방식
graph = [[] for _ in range(3)]
graph[0].append((1,7))
graph[0].append((2,5))
graph[1].append((0,7))
graph[2].append((0,5))


# DFS 예
def dfs(graph,v, visited):
  visited[v] = True
  print(v, end = ' ')
  for i in graph[v]:
    if not visited[i]:
      dfs(graph,i,visited)

graph = [
  [],
  [2,3,8],
  [1,7],
  [1,4,5],
  [3,5],
  [3,4],
  [7],
  [2,6,8],
  [1,7]
]

visited = [False] * 9
dfs(graph,1,visited)

# BFS 예
from collections import deque
def bfs(graph, start, visited):
  queue = deque([start])
  visited[start] = True
  while queue:
    v = queue.popleft()
    print(v, end = ' ')
    for i in graph[v]:
      if not visited[i]:
        queue.append(i)
        visited[i] = True
        
graph = [
  [],
  [2,3,8],
  [1,7],
  [1,4,5],
  [3,5],
  [3,4],
  [7],
  [2,6,8],
  [1,7]
]

visited = [False] * 9
bfs(graph,1,visited)
```

- 음료수 얼려 먹기 : 구멍 뚫린 부분끼리 상하좌우 붙어있으면 서로 연결되어 아이스크림 생성 가능
> 입력

- 첫 줄에 얼음 틀에 세로 n, 가로 m
- 둘째 줄에 n+1 번째 줄까지 얼음 틀 형태
- 구멍 뚫린부분 0, 아닌부분 1


> 출력

- 한번에 만들 수 있는 아이스크림의 개수 출력

```python
n,m  = map(int, input().split())
graph = []
for i in range(n):
  graph.append(list(map(int, input())))

def dfs(x,y):
  if x<=-1 or x>=n or y<=-1 or y>=m :
    return False
  if graph[x][y] == 0:
    graph[x][y] = 1
    def(x-1,y)
    def(x,y-1)
    def(x+1,y)
    def(x,y+1)
    return True
  return False

result = 0
for i in range(n):
  for j in range(m):
    if dfs(i,j) == True:
      result += 1
print(result)

```

- 미로 탈출 : 위치 (1,1)이고 출구는 (n,m) 위치 존재, 한번에 한 칸식만 이동 가능

> 입력
- 첫째 줄에 두 정수 n,m
- n개 줄에 m 개 정수로 미로 정보 주어짐
- 시작칸과 마지막 칸은 항상 1


> 출력

- 첫재 줄에 최소 이동칸의 개수 출력

```python
from collections import deque
n,m = map(int, input().split())
graph = []
for i in range(n):
  graph.append(list(map(int, input())))

dx = [-1,1,0,0]
dy = [0,0,-1,1]

def bfs(x,y):
  queue = deque()
  queue.append((x,y))
  while queue:
    x,y = queue.popleft()
    for i in range(4):
      nx = x+dx[i]
      ny = y+dy[i]
      if nx<0 or ny <0 or nx>=n or ny>=m:
        continue
      if graph[nx][ny] == 0:
        continue
      if graph[nx][ny] ==1:
        graph[nx][ny] = graph[x][y] +1
        queue.append((nx,ny))
       
  return graph[n-1][m-1]
print(bfs(0,0))
```

