## 1. 포인트 카드 (백준)

### 문제
1. 각 카드에는 2N개의 칸이 있고, 그 중 N개 이상의 칸에 당첨 도장이 있어야 경품과 교환 가능
2. JOI 군은 M장의 카드 중, 최소한 M-1개의 카드를 경품으로 바꾸어야함.
3. 당첨 도장이 부족한(N개보다) 카드는 **1엔을 내고** 꽝 도장을 당첨 도장으로 바꿔야한다. 

4. 현재 당첨 도장 수: 각 카드에 찍힌 당첨 도장 수 Ai와 꽝 도장 수 Bi가 주어진다. 
    - 만약 `Ai >= N` 인 카드들은 추가 비용 없이 바로 경품으로 교환할 수 있다.
    - `Ai < N` 인 카드들은 `N - Ai` 개의 당첨 도장을 더 찍어야 경품으로 교환 가능하다.. 

5. 당첨 도장이 부족한 카드들 중, 비용이 적게 드는 (**=부족한 당첨 도장 개수가 작은 순서대로**) 카드부터 꽝 도장을 바꾸어야한다. 

### 풀이 로직
1) Ai >= N 이면 바로 경품으로 교환
2) Ai < N 이면 N - Ai개의 꽝 도장을 바꾸는데, **이때 최소한의 도장 교환이 필요하도록**

```python

def solve():
    # 입력
    N, M = map(int, input().split())
    card = [] #리스트로 

    # M개의 카드 입력
    for _ in range(M):
        A, B = map(int, input().split()) #A는 당첨 도장 개수, B는 꽝 도장 개수
        card.append((A,B)) #카드 리스트에 추가 

    # 바꿔야하는 최소한의 경품 개수
        prizes = M - 1

    # 당첨 도장이 N개 이상 => 바로 교환 가능할 때 
    first_prize = 0 
    changes = [] #당첨으로 바꿔야하는 도장의 개수 리스트에 저장

    for A,B in card:
        if A >= N:
        first_prizes += 1 #바로 경품으로 교환
        else:
        changes.append(N-A) #부족한 당첨 도장 수 N-A개 리스트에 저장 

    # 바로 M-1개를 채웠을 때=> 추가비용X
    if first_prizes >= prizes: 
        print(0)
        return

    # 부족한 당첨 도장 수 정렬하기 
    changes.sort() #오름차순

    # 비용 계산 
    cost = sum(chanes[:prizes-first_prizes])
    print(cost)

solve()
```


## PCCP 기출문제 1번: 동영상 재생기

동영상의 길이를 나타내는 문자열 `video_len`, 기능이 수행되기 직전의 재생위치를 나타내는 문자열 `pos`, 오프닝 시작 시각을 나타내는 문자열 `op_start`, 오프닝이 끝나는 시각을 나타내는 문자열 `op_end`, 사용자의 입력을 나타내는 1차원 문자열 배열 `commands`가 매개변수로 주어집니다. 이때 사용자의 입력이 모두 끝난 후 동영상의 위치를 "mm:ss" 형식으로 return 하도록 `solution` 함수를 완성해 주세요.

```python
def solution(video_len, pos, op_start, op_end, commands):
    # 시간을 초 단위로 변환하는 함수
    def time_to_sec(timestamp: str):
        minute, second = map(int, timestamp.split(":"))  # 콜론을 기준으로 분과 초를 분리
        return minute * 60 + second

    # 초를 다시 '분:초' 형식으로 변환하는 함수
    def sec_to_time(seconds: int):
        minute = seconds // 60
        second = seconds % 60
        return f"{minute:02}:{second:02}" #두자리 숫자로 포맷팅

    # 초 단위로 변환
    pos_sec = time_to_sec(pos)
    video_len_sec = time_to_sec(video_len)
    op_start_sec = time_to_sec(op_start)
    op_end_sec = time_to_sec(op_end)

    for user_input in commands:
        if user_input == 'next':
            if op_start_sec <= pos_sec <= op_end_sec:
                pos_sec = op_end_sec
            pos_sec += 10  # 현재 위치에서 10초 후로 이동
            
        elif user_input == 'prev':  # 10초 전으로 이동
            if op_start_sec <= pos_sec <= op_end_sec:
                pos_sec = op_end_sec
            pos_sec -= 10

        # 영상의 마지막 위치는 동영상의 길이와 같습니다.
        if pos_sec > video_len_sec:
            pos_sec = video_len_sec
        #현재 위치가 10초 미만인 경우 영상의 처음 위치로 이동
        if pos_sec < 0:
            pos_sec = 0

    # 최종 위치가 특정 구간 안에 있다면 끝 위치로 이동
    if op_start_sec <= pos_sec <= op_end_sec:
        pos_sec = op_end_sec

    # 최종 위치를 '분:초' 형식으로 변환하여 반환
    return sec_to_time(pos_sec)
```

## 명예의 전당

<문제>
"명예의 전당"이라는 TV 프로그램에서는 매일 1명의 가수가 노래를 부르고, 시청자들의 문자 투표수로 가수에게 점수를 부여합니다. 매일 출연한 가수의 점수가 지금까지 출연 가수들의 점수 중 상위 k번째 이내이면 해당 가수의 점수를 명예의 전당이라는 목록에 올려 기념합니다. 즉 프로그램 시작 이후 초기에 k일까지는 모든 출연 가수의 점수가 명예의 전당에 오르게 됩니다. k일 다음부터는 출연 가수의 점수가 기존의 명예의 전당 목록의 k번째 순위의 가수 점수보다 더 높으면, 출연 가수의 점수가 명예의 전당에 오르게 되고 기존의 k번째 순위의 점수는 명예의 전당에서 내려오게 됩니다.

이 프로그램에서는 매일 "명예의 전당"의 최하위 점수를 발표합니다. 예를 들어, k = 3이고, 7일 동안 진행된 가수의 점수가 [10, 100, 20, 150, 1, 100, 200]이라면, 명예의 전당에서 발표된 점수는 아래의 그림과 같이 [10, 10, 10, 20, 20, 100, 100]입니다.

> 명예의 전당 목록의 점수의 개수 k, 1일부터 마지막 날까지 출연한 가수들의 점수인 score가 주어졌을 때, 매일 발표된 명예의 전당의 최하위 점수를 return하는 solution 함수를 완성해주세요.


```py
def solution(k, score):
    fame = [] #명예의 전당에 들어가는 리스트
    result = [] #스코어 결과값 들어가는 리스트
    for i in score:
        fame.append(i)
        fame.sort(reverse=True)#잘한것부터 내림차순정렬
        fame_slice = fame[:k] #상위 k번째까지 슬라이스
        result.append(fame_slice[-1])
    
    return result

```