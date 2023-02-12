# [Programmers 17683](https://school.programmers.co.kr/learn/courses/30/lessons/17683) ��ݱװ�

## ���� ���

1. ��ε�� �Ǻ��� ���Ǵ� ���� C, C#, D, D#, E, F, F#, G, G#, A, A#, B `12��`

2. �� ���� 1�п� 1���� ���, ������ �ݵ�� ó������ ����Ǹ� ���� ���̺��� ����� �ð��� �� ���� ������ ���� ���� ó������ �ݺ��ؼ� ���

## ��ǥ

- ������ ��ġ�ϴ� ������ ���� ���� ������ �������� `����� �ð��� ���� �� ���� ����`�� ��ȯ, ����� �ð��� ���� ��� `���� �Էµ� ���� ����`�� ��ȯ
- ������ ��ġ�ϴ� ������ ���� ������ `��(None)��`�� ��ȯ

### ���� �Է�

```
m="CC#BCC#BCC#BCC#B"
musicinfos=["03:00,03:30,FOO,CC#B", "04:00,04:08,BAR,CC#BCC#BCC#B"]
```

### ���� ���

```
"FOO"
```

- BAR�� ��� �ݺ��� 8���� ����Ƿ� ���� ���� x
- FOO�� ��� 30�� ���� ���ӵǹǷ� ���� ���� o

## Ǯ��

- `����`

### �ֿ� ��

#### 1. �ð� -> �� ��

```python
def time_calculator(time):
    h, m = time.split(":")
    return int(h) * 60 + int(m)
```

#### 2. # ��ü

```python
def map_syllables(m):
    return m.replace("C#", "1").replace("D#", "2").replace("F#", "3").replace("G#", "4").replace("A#", "5")
```

## ��ü �ڵ�

```python
def time_calculator(time):
    h, m = time.split(":")
    return int(h) * 60 + int(m)


def map_syllables(m):
    return m.replace("C#", "1").replace("D#", "2").replace("F#", "3").replace("G#", "4").replace("A#", "5")


def solution(m, musicinfos):
    answer = {"name": "", "whole_music": ""}
    for music in musicinfos:
        start, end, name, part = music.split(",")
        m, part = map_syllables(m), map_syllables(
            part)  # map both test and vaildation
        part *= 1439  # max
        whole_music = part[:time_calculator(
            end)-time_calculator(start)]  # duration
        if m in whole_music:
            if len(whole_music) > len(answer["whole_music"]):
                answer["name"], answer["whole_music"] = name, whole_music
    if (answer["name"]):
        return answer["name"]
    else:
        return "(None)"
```
