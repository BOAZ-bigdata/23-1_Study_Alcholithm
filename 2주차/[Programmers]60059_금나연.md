# [Programmers 60059] �ڹ���� ����

## ���� ���

1. `N\*N` �ڹ���� `M*M` ����

- key�� lock�� ���Ҵ� `0` �Ǵ� `1`�� �̷���� ����
  - `0`�� Ȩ �κ�, `1`�� ���� �κ�

2. �ڹ����� Ȩ �κп� ������ ���� �κ��� ��Ȯ�ϰ� ���µ��� ���踦 ȸ���ϰ� �����̵�

## ��ǥ

��������(True), �Ұ�������(False)

### ���� �Է�

```
key=[
    [0, 0, 0],
    [1, 0, 0],
    [0, 1, 1]
    ]

lock=[
    [1, 1, 1],
    [1, 1, 0],
    [1, 0, 1]
    ]
```

### ���� ���

```
True
```

## Ǯ��

- `Simulation`

### 1Ʈ

- ������� ������ �����ؼ� bfs�� �������� ���纼�� ����
  => �������� �����ؼ� �ð� �ȿ� ���� �� ���� �Ѿ��� ��
  => �غôµ� �� �� ��,, �� �� ����

### 2Ʈ

- ������
  - ���߹迭 ȸ����Ű�� �� ã�ƺ�.. ������..
- ��ģ for loop.. �ʹ� �������� �Լ�ȭ�� �� ���� ��

### �ֿ� ��

#### 1. �ڹ��� Ȯ��

```
[0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0, 0]
[0, 0, 1, 1, 1, 0, 0]
[0, 0, 1, 1, 0, 0, 0]
[0, 0, 1, 0, 1, 0, 0]
[0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0, 0]
```

- �� �鿡 n+2(m-1)���� Ȯ��

```python
def expand_lock(lock, m):
    k = n+2*(m-1)
    expanded_lock = [[0]*(k) for _ in range(k)]
    for i in range(len(lock)):
        for j in range(len(lock)):
            expanded_lock[i+(m-1)][j+(m-1)] = lock[i][j]
    return expanded_lock
```

#### 2. �ڹ���� ���踦 attach/detach

```python
def attach(i, j, lock, key):
    for x in range(m):
        for y in range(m):
            lock[i+x][j+y] += key[x][y]


def detach(i, j, lock, key):
    for x in range(m):
        for y in range(m):
            lock[i+x][j+y] -= key[x][y]
```

- lock�� key�� �ش� �κ� ������ ����

#### 3. �ڹ���� ���谡 ���´��� check

```python
def check(attached_lock):
    for x in range(m-1, n+m-1):
        for y in range(m-1, n+m-1):
            if attached_lock[x][y] != 1:
                return False
    return True
```

- ���� 1�� ��� �� ����
  - 0�� ��� ������
  - 2�� ��� �浹

## ��ü �ڵ�

```python
def turn_key(key):
    return list(zip(*key[::-1]))

def check(attached_lock):
    for x in range(m-1, n+m-1):
        for y in range(m-1, n+m-1):
            if attached_lock[x][y] != 1:
                return False
    return True

def expand_lock(lock, m):  # m: size of key
    k = n+2*(m-1)
    expanded_lock = [[0]*(k) for _ in range(k)]
    for i in range(len(lock)):
        for j in range(len(lock)):
            expanded_lock[i+(m-1)][j+(m-1)] = lock[i][j]
    return expanded_lock


def attach(i, j, lock, key):
    for x in range(m):
        for y in range(m):
            lock[i+x][j+y] += key[x][y]


def detach(i, j, lock, key):
    for x in range(m):
        for y in range(m):
            lock[i+x][j+y] -= key[x][y]


def solution(key, lock):
    global n, m
    n, m = len(lock), len(key)

    expanded_lock = expand_lock(lock, m)
    for _ in range(4):
        for i in range(n+m-1):
            for j in range(n+m-1):
                attach(i, j, expanded_lock, key)
                if check(expanded_lock):
                    return True
                detach(i, j, expanded_lock, key)
        key = turn_key(key)
    return False
```

## 1Ʈ ��

### 1. key�� 1 ��ġ�� lock�� 0 ��ġ�� �ľ��Ѵ�

```python
def findChar(lock, char):
    locations = []
    for i in range(len(lock)):
        for j in range(len(lock)):
            if lock[i][j] == char:
                locations.append((i, j))
    return locations


def makeDisplacement(locations):
    std_x, std_y = locations[0][0], locations[0][1]
    displacements = []
    for i in range(len(locations)):
        displacements.append((locations[i][0]-std_x, locations[i][1]-std_y))
    return displacements
```

- findChar()�� �������� location�� ��ȯ�ϰ�, makeDisplacement()�� ������� ��ġ(����)�� ��ȯ�Ѵ�.
  ex)

```
0 1 0
1 0 0
1 0 0
```

- `findChar(key, 1)` = [(0, 1), (1, 0), (2, 0)]
- `makeDisplacement(findChar(key,1))` = [(0, 0), (-1, 1), (-2, 1)]

### 2. �˸´��� üũ

```python
def check(expanded_lock, key_unlock_displacements, key_crash_displacements, lock_unlock_locations, lock_crash_locations, std_1location, key_unlock_locations):
    # unlock
    for current_x, current_y in lock_unlock_locations:
        can_unlock = []
        for trial in key_crash_displacements:
            if m-1 <= current_x+trial[0] < n+m-1 and m-1 <= current_y+trial[1] < n+m-1:
                can_unlock.append((current_x+trial[0], current_y+trial[1]))
        # currents displacement isn't same with unlock locations of lock(0)
        can_unlock.sort()
        if can_unlock != lock_unlock_locations:
            continue
        else:
            return True

```

```
for x, y in (lock�� 0 ��ġ):
    for ���� in (key�� 1 ���� list):
        x, y�� ������ ���ؼ� ���� lock�� �ش��ϴ� index range�� ���� �� can_unlock list�� append
    if (can_unlock list�� lock�� 0 ��ġ�� ��Ȯ�ϰ� ��ġ):
        return True
    else: continue
```

### ��ü �ڵ�(54.0/100.0)

```python
def turn_key(key):
    return list(zip(*key[::-1]))

def check(key_crash_displacements, lock_unlock_locations):
    # unlock
    for current_x, current_y in lock_unlock_locations:
        can_unlock = []
        for trial in key_crash_displacements:
            if m-1 <= current_x+trial[0] < n+m-1 and m-1 <= current_y+trial[1] < n+m-1:
                can_unlock.append((current_x+trial[0], current_y+trial[1]))
        # currents displacement isn't same with unlock locations of lock(0)
        can_unlock.sort()
        if can_unlock != lock_unlock_locations:
            continue
        else:
            return True


def findChar(lock, char):
    locations = []
    for i in range(len(lock)):
        for j in range(len(lock)):
            if lock[i][j] == char:
                locations.append((i, j))
    return locations


def makeDisplacement(locations):
    std_x, std_y = locations[0][0], locations[0][1]
    displacements = []
    for i in range(len(locations)):
        displacements.append((locations[i][0]-std_x, locations[i][1]-std_y))
    return displacements


def expand_lock(lock):  # m: size of key
    k = n+2*(m-1)
    expanded_lock = [[0]*(k) for _ in range(k)]
    for i in range(len(lock)):
        for j in range(len(lock)):
            expanded_lock[i+(m-1)][j+(m-1)] = lock[i][j]
    return expanded_lock


def solution(key, lock):
    global n, m
    n, m = len(lock), len(key)

    expanded_lock = expand_lock(lock)

    for _ in range(4):

        key = turn_key(key)
        key_crash_displacements = makeDisplacement(findChar(key, 1))
        # displacement needs a starting point(initializes to (0,0))
        lock_unlock_locations = [(x, y) for x, y in findChar(
            expanded_lock, 0) if m-1 <= x < n+m-1 and m-1 <= y < n+m-1]

        if check(key_crash_displacements,  lock_unlock_locations):
            return True
    return False
```
