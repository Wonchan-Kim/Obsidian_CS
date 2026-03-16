# Cache Coherence — Shared Memory Multiprocessors

> [!summary]
> 이 슬라이드는 **왜 cache coherence가 필요한지**를 설명하는 배경 슬라이드다.  
> shared memory는 프로그래밍이 쉽지만, 각 프로세서가 자기 캐시를 가지면 같은 데이터의 여러 사본이 생길 수 있다.  
> 따라서 **한 프로세서의 write가 다른 프로세서에게도 올바르게 보이도록** 하려면 hardware cache coherence가 필요하다.

---

## 0. 제목 슬라이드

# Cache Coherence

이 강의 전체 주제가 **캐시 일관성(cache coherence)** 이라는 뜻이다.

왜 이게 중요한가?

멀티프로세서/멀티코어에서 각 코어가 자기 캐시를 가지면 성능은 좋아지지만, 문제가 하나 생긴다.

같은 메모리 주소를 여러 코어가 각자 캐시에 따로 들고 있으면, **어느 값이 진짜 최신 값인지** 맞춰야 한다.

이 문제를 해결하는 것이 **cache coherence**이다.

즉 이 다음 슬라이드부터는  
여러 프로세서가 메모리를 공유하는 시스템에서, 각자의 캐시가 서로 모순되지 않게 만드는 방법을 배우게 된다.

> [!important]
> **cache coherence의 핵심 질문**  
> 여러 캐시에 같은 주소의 복사본이 있을 때,  
> **write 이후 어떤 값이 최신값인지 어떻게 모두가 일관되게 보게 할 것인가?**

---

## 1. Shared Memory Multiprocessors

이 제목은 말 그대로 **메모리를 공유하는 멀티프로세서 시스템**을 뜻한다.

단어를 나누면:

- **Shared Memory**: 메모리가 공유됨
- **Multiprocessors**: 프로세서가 여러 개 있음

즉 여러 프로세서가 **같은 주소 공간(shared address space)** 을 보고 있는 구조라는 뜻이다.

> [!note]
> shared-memory system에서는 프로그래머가  
> "이 메모리는 P0 전용", "저 메모리는 P1 전용"  
> 이런 식으로 생각하지 않고,  
> **하나의 공통 주소 공간을 여러 프로세서가 공유한다**고 생각할 수 있다.

---

## 2. All processors can access all memory

이것은 shared-memory의 가장 핵심 정의이다.

의미:

시스템 안의 어떤 프로세서든, **모든 메모리 위치에 접근할 수 있다.**

예를 들어 메모리에 변수 `X`가 있으면:

- `P0`도 `X`를 읽고 쓸 수 있다.
- `P1`도 `X`를 읽고 쓸 수 있다.
- `P2`도 `X`를 읽고 쓸 수 있다.

### 왜 중요한가

이 말은 프로그래머 입장에서

- "이 변수는 P0 전용 메모리다"
- "이 변수는 P1만 접근 가능하다"

이런 식으로 생각하지 않아도 된다는 뜻이다.

그냥 **하나의 큰 공유 주소 공간**이 있다고 생각하면 된다.

따라서 프로그램은 자연스럽게 다음처럼 작성된다.

- 여러 스레드가 같은 배열에 접근
- 같은 큐(queue)에 접근
- 같은 lock 변수에 접근
- 같은 counter에 접근

그래서 프로그래밍은 편해지지만, 바로 이 때문에 coherence 문제가 생긴다.

> [!example]
> 예를 들어 `counter`라는 변수가 shared memory에 있으면  
> `P0`, `P1`, `P2` 모두 같은 `counter`를 읽고 쓸 수 있다.  
>  
> 즉 "공유"라는 것은 편리하지만, 동시에  
> **누가 언제 어떤 값을 보느냐**가 중요해진다는 뜻이다.

---

## 3. Processors share memory resources, but can operate independently

이 문장은 두 가지를 동시에 말한다.

### (1) share memory resources

프로세서들은 메모리를 공유한다.

즉 다음 같은 자원을 함께 쓴다.

- 같은 DRAM
- 같은 주소 공간
- 경우에 따라 같은 shared cache
- 같은 interconnect

### (2) but can operate independently

그렇다고 한 프로세서가 다른 프로세서의 허락을 받고 움직이는 것은 아니다.

각 프로세서는 독립적으로 다음을 수행할 수 있다.

- instruction fetch
- execute
- load/store
- cache access

즉 `P0`가 뭔가 계산하는 동안, `P1`도 동시에 다른 계산을 할 수 있다.

### 왜 중요한가

바로 여기서 **병렬성(parallelism)** 이 나온다.

- 서로 독립적으로 실행 가능 → 성능 향상 가능
- 하지만 같은 메모리를 공유 → 충돌 / 동기화 / 일관성 문제 발생

즉 이 문장은 shared-memory parallelism의 핵심 trade-off를 말한다.

**독립적으로 빠르게 돌고 싶지만, 메모리는 공유하니까 서로 영향 줄 수 있다.**

> [!important]
> shared-memory multiprocessor의 핵심 trade-off:
>
> - **병렬성**을 얻고 싶다
> - 하지만 **공유 데이터** 때문에 서로 영향을 준다
> - 그래서 coherence와 synchronization이 필요해진다

---

## 4. One processor’s memory changes are seen by all other processors

이 bullet은 shared memory에서 반드시 만족되어야 하는 기대를 말한다.

의미:

한 프로세서가 메모리 값을 바꾸면, 다른 프로세서들도 **결국 그 변화를 볼 수 있어야 한다.**

예를 들어 `P0`가

    flag = 1;

를 실행했으면, `P1`도 나중에 `flag`를 읽을 때 `1`을 볼 수 있어야 한다.

또는 `P0`가

    buffer[i] = 99;

를 저장했으면, `P1`이 같은 위치를 읽을 때 결국 `99`를 봐야 한다.

### 왜 "결국"이라는 표현이 중요한가

실제 하드웨어에서는 각자 캐시가 있어서 값이 **즉시 전체 시스템에 동시에 보이는 것처럼** 동작하지는 않는다.

예를 들어:

- `P0`의 cache에는 새 값이 있음
- `P1`의 cache에는 옛날 값이 남아 있음

이럴 수 있다.

그래서 하드웨어가 coherence protocol을 사용해서  
**최종적으로 다른 프로세서들도 올바른 최신 값을 보게 만들어야 한다.**

즉 이 bullet은:

- **프로그래머 관점의 기대**
- **하드웨어가 만족시켜야 하는 조건**

을 말한다.

> [!warning]
> shared memory라고 해서 모든 코어가 항상 같은 순간에 완전히 동일한 값을 본다고 단순하게 생각하면 안 된다.  
> 실제로는 캐시 때문에 **일시적으로 서로 다른 복사본**을 가질 수 있다.  
> coherence는 이 상황을 제어해서 **올바른 최신 값이 eventually 보이도록** 만든다.

---

## 5. Easier to program

이것은 shared-memory 모델의 큰 장점이다.

의미:

분산 메모리 시스템보다 보통 **프로그래밍하기가 더 쉽다.**

왜 쉬운지는 아래 두 개의 서브 bullet이 설명한다.

---

## 5.1 Communication through shared memory

프로세서끼리 통신하려면 shared-memory 모델에서는 그냥 같은 메모리 변수를 사용하면 된다.

예:

- 한 스레드가 queue에 데이터를 넣음
- 다른 스레드가 같은 queue에서 데이터를 꺼냄

또는

- `P0`가 `result[i]`를 계산해서 저장
- `P1`이 그 값을 읽어서 다음 연산 수행

즉 통신 자체가 "메시지 보내기"가 아니라  
**같은 메모리를 읽고 쓰는 것**으로 표현된다.

### 분산 메모리와 비교

분산 메모리에서는 보통 다음 같은 것을 써야 한다.

- `send`
- `receive`
- explicit message passing

하지만 shared memory에서는:

- 그냥 변수 쓰기
- 그냥 배열 읽기

처럼 보인다.

그래서 프로그래머 입장에서는 훨씬 자연스럽고 쉽다.

> [!note]
> shared memory에서는 **communication = shared variable access** 처럼 보인다.  
> 즉 프로세서 사이 통신이 "특별한 네트워크 메시지"가 아니라  
> 그냥 **메모리 읽기/쓰기**로 표현된다.

---

## 5.2 Synchronization through locks stored in shared memory

통신만 중요한 것이 아니라, 여러 프로세서가 동시에 같은 데이터를 건드리면 충돌하므로 **동기화(synchronization)** 가 필요하다.

shared memory에서는 **lock 자체도 메모리에 저장된 변수**로 둘 수 있다.

예를 들어:

    lock(L);
    counter++;
    unlock(L);

여기서 `L`도 사실 shared memory 안에 있는 어떤 값이다.

즉:

- communication도 shared memory 사용
- synchronization도 shared memory 사용

이것이 shared-memory programming model의 큰 장점이다.

### 왜 중요한가

프로그래머 입장에서는 병렬 프로그램을 쓸 때

- 데이터도 메모리 안에 있고
- lock도 메모리 안에 있고
- condition variable도 메모리 안에 있고
- flags도 메모리 안에 있다

즉 거의 모든 것이 같은 주소 공간 안에서 처리된다.

그래서 모델이 단순하다.

> [!important]
> shared memory 모델의 큰 장점:
>
> - **communication**도 memory로 한다
> - **synchronization**도 memory로 한다
>
> 즉 프로그래밍 모델이 하나로 통일된다.

---

## 6. Need cache coherence in hardware

이 bullet이 이 강의의 핵심 진입점이다.

shared memory가 편한 대신, 그것을 실제 하드웨어에서 성립시키려면  
**cache coherence가 반드시 필요하다.**

### 왜 필요한가

각 프로세서가 캐시를 하나도 안 쓰고 매번 메모리만 직접 보면 일관성 문제는 비교적 단순하다.

하지만 그러면 성능이 너무 느리다.

그래서 실제로는:

- `P0` has cache
- `P1` has cache
- `P2` has cache

각자 로컬 캐시를 사용한다.

이제 같은 변수 `X`를 `P0` cache와 `P1` cache가 동시에 들고 있을 수 있다.

그 상태에서 `P0`가 `X = 5`로 바꾸면, `P1` cache 안에 있는 옛날 `X`는 어떻게 할 것인가?

바로 이 문제를 해결하는 하드웨어가 **cache coherence**이다.

### 더 깊은 의미

프로그래머는 그냥 shared memory라고 생각하고 코드를 짜지만,  
하드웨어는 내부적으로 다음 같은 일을 해야 한다.

- invalidate
- write-back
- read miss 처리
- ownership 관리
- sharer 추적

즉 shared memory의 "쉬운 프로그래밍 모델"은 사실  
**하드웨어가 복잡한 coherence를 해주기 때문에 가능한 것이다.**

> [!danger]
> 성능을 위해 캐시를 쓰면,  
> 같은 주소의 **복사본이 여러 군데 존재**하게 된다.  
>  
> 이때 coherence가 없으면:
>
> - 어떤 코어는 옛날 값
> - 어떤 코어는 새 값
>
> 을 보는 모순이 생길 수 있다.

---

## 7. Need interconnection network between all processors and all memory

이 bullet은 물리적인 연결 구조를 말한다.

의미:

모든 프로세서와 모든 메모리를 연결해주는  
**인터커넥션 네트워크(interconnection network)** 가 필요하다.

왜냐하면 shared memory라고 말하려면  
모든 프로세서가 모든 메모리에 접근할 수 있어야 하기 때문이다.

### interconnection network란

프로세서와 메모리, 캐시 사이를 이어주는 하드웨어 통신 구조이다.

예:

- bus
- crossbar
- ring
- mesh
- NoC (network-on-chip)

### 왜 그냥 선 몇 개로 끝나지 않는가

프로세서 수가 많아질수록 문제가 생긴다.

- 여러 프로세서가 동시에 메모리 접근
- coherence 메시지도 오감
- invalidate도 오감
- data reply도 오감

즉 단순히 "연결되어 있다"만으로 끝나는 것이 아니라,  
**충분한 대역폭과 적절한 구조**가 필요하다.

그래서 뒤에서 snooping이 왜 scalability에 약한지,  
directory가 왜 필요한지와 연결된다.

> [!note]
> shared memory는 단순히 "모두 같은 메모리를 본다"는 추상적 개념이 아니라,  
> 실제 하드웨어에서는  
> **processor-cache-memory를 연결하는 통신 구조**가 꼭 필요하다.

---

## 8. Two Types:

이제 shared-memory multiprocessor를 크게 두 종류로 나누는 것이다.

- UMA
- NUMA

이 구분은  
**모든 메모리 접근이 같은 비용이냐, 아니냐**를 기준으로 한다.

---

## 9. Uniform Memory Architectures (UMA): e.g., Symmetric Multiprocessors

UMA는 **Uniform Memory Access**의 뜻이다.

즉:

어떤 프로세서가 어떤 메모리 위치를 접근하든,  
메모리 접근 시간/비용이 거의 동일하다.

### 왜 uniform인가

메모리가 시스템 관점에서 거의 "한 덩어리"처럼 보이고,  
모든 CPU가 비슷한 거리/비용으로 접근하기 때문이다.

예전의 **SMP (Symmetric Multiprocessor)** 가 대표 예시이다.

### Symmetric의 의미

각 프로세서가 메모리에 대해 대체로 대칭적인 위치를 가진다는 뜻이다.

즉 한 프로세서가 특별히 더 가까운 메모리를 가지지 않는다.

### 장점

- 프로그래밍 모델이 단순하다.
- 메모리 배치 고민이 적다.
- 시스템 구조를 이해하기 쉽다.

### 단점

프로세서 수가 많아질수록 모두가 같은 메모리 시스템과 인터커넥트를 공유하므로 병목이 생긴다.

즉 small/medium scale에는 좋지만,  
크게 키우면 대역폭과 coherence broadcast 때문에 힘들어진다.

> [!example]
> UMA에서는 보통
>
> - `CPU0`가 `X` 읽는 시간
> - `CPU1`가 `X` 읽는 시간
> - `CPU2`가 `X` 읽는 시간
>
> 이 거의 비슷하다고 생각할 수 있다.

---

## 10. Non-Uniform Memory Architectures (NUMA): Access & latency to memory is different

NUMA는 **Non-Uniform Memory Access**이다.

즉:

메모리 접근 시간이 **위치에 따라 다르다.**

어떤 메모리는 "가까운 메모리", 어떤 메모리는 "먼 메모리"가 된다.

### 왜 non-uniform인가

보통 시스템이 여러 노드로 나뉘고, 각 노드가 자기 **local memory**를 가지기 때문이다.

예를 들어:

- `P0`가 자기 노드 메모리 접근 → 빠름
- `P0`가 다른 노드 메모리 접근 → 느림

이렇게 된다.

### shared memory와 모순인가?

아니다. NUMA도 여전히 shared memory이다.

즉 모든 프로세서가 전체 주소 공간에 접근 가능하다.  
다만 **접근 비용이 다를 뿐**이다.

그래서 shared-memory multiprocessor 안에서도 규모가 커지면 NUMA가 많이 등장한다.

### 왜 NUMA를 쓰는가

확장성 때문이다.

- 메모리를 분산시켜 대역폭을 늘리고
- local traffic / remote traffic을 분리하고
- 더 큰 시스템을 만들기 쉽다

하지만 단점은:

- 원격 메모리 접근이 느리다
- 성능이 데이터 배치에 민감하다
- 프로그래밍과 성능 최적화가 어려워질 수 있다

> [!important]
> **NUMA도 shared memory다.**  
>  
> 차이는:
>
> - UMA: 어디를 읽어도 거의 비슷한 비용
> - NUMA: 어떤 메모리는 가깝고, 어떤 메모리는 멀다

---

## 11. 슬라이드 전체가 전달하는 큰 흐름

이 슬라이드는 사실 다음 논리를 만들고 있다.

### 1단계

우리는 shared-memory multiprocessor를 원한다.  
왜냐하면 프로그래밍이 쉽기 때문이다.

### 2단계

그런데 각 프로세서가 독립적으로 실행하면서도 같은 메모리를 공유하면,  
한 프로세서의 변경이 다른 프로세서에게 보여야 한다.

### 3단계

성능 때문에 각자 캐시를 써야 한다.

### 4단계

그러면 shared data가 여러 캐시에 복사될 수 있으므로  
hardware cache coherence가 필요하다.

### 5단계

또 모든 프로세서와 메모리를 연결하는 interconnect도 필요하다.

### 6단계

이런 시스템은 크게 UMA와 NUMA로 나뉜다.

즉 이 슬라이드는 앞으로 배울 coherence protocol들이  
어떤 문제를 해결하려고 등장했는지 배경을 깔아주는 슬라이드다.

> [!summary]
> 이 슬라이드의 전체 스토리:
>
> 1. shared memory는 프로그래밍이 쉽다  
> 2. 하지만 여러 프로세서가 동시에 접근한다  
> 3. 성능 때문에 각자 캐시도 쓴다  
> 4. 그러면 같은 데이터의 여러 사본이 생긴다  
> 5. 그래서 cache coherence가 필요하다

---

## 12. 예시로 다시 보기

### 예시 1: shared memory communication

`P0`가 이렇게 쓴다.

    data = 42;
    flag = 1;

`P1`은:

    while (flag == 0) {}
    print(data);

이 코드는 shared memory라서 가능하다.

- `flag`도 shared
- `data`도 shared

하지만 이 코드가 제대로 동작하려면  
`P0`의 write가 `P1`에게 보여야 하므로 coherence가 필요하다.

> [!example]
> `flag`가 먼저 보이고 `data`가 안 보이면 문제가 생길 수 있다.  
> 즉 shared memory에서는 단순히 "같은 변수 접근"이 가능하다는 것뿐 아니라,  
> **그 변화가 다른 코어에게 올바르게 전달되는 것**이 중요하다.

---

### 예시 2: synchronization through locks

    lock(L);
    sum += x;
    unlock(L);

여기서 `L`도 메모리 안의 값이다.

`P0`, `P1`, `P2`가 모두 같은 `L`을 보고  
누가 critical section에 들어갈지 결정한다.

이 역시 shared memory 모델의 장점이지만,  
동시에 lock 변수도 여러 캐시에 복사될 수 있으므로 coherence가 매우 중요하다.

> [!important]
> lock 변수는 보통 아주 자주 읽히고/쓰이는 shared data이므로  
> coherence가 제대로 안 되면 synchronization 자체가 깨진다.

---

### 예시 3: UMA vs NUMA

#### UMA

- `CPU0`가 `X` 읽음: `100ns`
- `CPU1`가 `X` 읽음: `100ns`
- `CPU2`가 `X` 읽음: `100ns`

거의 비슷하다.

#### NUMA

- `CPU0`가 자기 노드 메모리 `X` 읽음: `80ns`
- `CPU0`가 다른 노드 메모리 `Y` 읽음: `180ns`

이렇게 차이가 난다.

> [!note]
> NUMA에서는 **어디에 데이터를 배치하느냐**가 성능에 직접 영향을 준다.  
> local memory를 많이 쓰면 빠르고, remote memory를 많이 쓰면 느려진다.

---

## 13. 이 슬라이드에서 꼭 잡아야 하는 포인트

### 포인트 1

shared memory는  
**"모두가 같은 주소 공간을 볼 수 있다"** 는 뜻이지,  
**"캐시 없이 그냥 한 메모리만 쓴다"** 는 뜻이 아니다.

### 포인트 2

프로그래밍이 쉬운 이유는  
**communication**과 **synchronization**을 둘 다 shared memory 위에서 표현할 수 있기 때문이다.

### 포인트 3

하지만 실제 하드웨어는 캐시 때문에  
**coherence가 반드시 필요하다.**

### 포인트 4

시스템 규모가 커지면 interconnect와 memory bandwidth가 중요해지고,  
그래서 UMA/NUMA 구분이 생긴다.

> [!tip]
> 시험에서 이 슬라이드를 설명하라고 하면,  
> 아래 흐름만 잡으면 된다:
>
> **shared memory의 장점 → 캐시 사용 → coherence 필요 → interconnect 필요 → UMA/NUMA 구분**

---

## 14. 시험용 한 줄 요약

- **All processors can access all memory**  
  모든 프로세서가 전체 주소 공간을 접근할 수 있다.

- **Processors share memory resources, but can operate independently**  
  메모리는 공유하지만 실행은 병렬로 독립적으로 진행된다.

- **One processor’s memory changes are seen by all other processors**  
  한 프로세서의 write는 다른 프로세서에게도 결국 보여야 한다.

- **Easier to program**  
  communication과 synchronization을 shared memory 위에서 표현할 수 있어 프로그래밍이 쉽다.

- **Need cache coherence in hardware**  
  각 프로세서의 캐시 사본들을 일관되게 유지하기 위해 coherence 하드웨어가 필요하다.

- **Need interconnection network**  
  모든 프로세서와 메모리를 연결하는 충분한 대역폭의 연결망이 필요하다.

- **UMA**  
  모든 메모리 접근 비용이 거의 동일하다.

- **NUMA**  
  메모리 접근 비용이 위치에 따라 다르다.

---

## 15. 다음 내용과의 연결

바로 다음 단계에서 보게 될 논리는 대체로 다음과 같다.

1. shared memory는 좋다.
2. 캐시도 필요하다.
3. 그런데 shared data가 여러 캐시에 복사되면 문제가 생긴다.
4. 그래서 coherence protocol이 필요하다.
5. snooping / directory 같은 방식이 나온다.
6. 규모가 커지면 UMA보다 NUMA와 directory가 중요해진다.

즉 이 슬라이드는 사실상

**왜 cache coherence를 배워야 하는가**

를 설명하는 배경 슬라이드다.

> [!summary]
> 한 줄로 끝내면:
>
> **Shared memory는 프로그래밍을 쉽게 만들지만, 각 프로세서가 자기 캐시를 가지는 순간 여러 사본이 생기므로 hardware cache coherence가 필수다.**!

# Interconnection Networks

> [!summary]
> 이 슬라이드는 shared-memory multiprocessor에서 **왜 interconnection network가 필요한지**, 그리고 **어떤 연결 방식들이 있는지**를 소개하는 슬라이드다.  
> 핵심은 단순히 "연결만 되면 된다"가 아니라, **어떤 topology를 쓰느냐에 따라 비용, 지연, 대역폭, 확장성, coherence 방식까지 달라진다**는 점이다.

---

## 1. 이 슬라이드의 역할

이 슬라이드는 shared-memory multiprocessor에서 왜 **interconnection network**가 필요한지, 그리고 어떤 연결 방식들이 있는지를 소개하는 슬라이드다.

앞 슬라이드에서 이미

- 모든 프로세서가 모든 메모리에 접근해야 하고
- 그러려면 interconnect가 필요하다

는 점을 배웠다.

이번 슬라이드는 그 말을 조금 더 구체적으로 바꾼 것이다.

즉 질문이 이렇게 바뀐다.

**"좋아, 연결은 해야 한다. 그런데 어떤 구조로 연결할 것인가?"**

> [!important]
> 이 슬라이드의 핵심은  
> **interconnect의 존재 자체**보다도,  
> **어떤 topology를 선택하느냐에 따라 시스템 성격이 달라진다**는 점이다.

---

## 2. In a shared memory MP, we need to connect different processors and memory modules

여기서 **MP**는 **multiprocessor**를 뜻한다.

즉 shared-memory multiprocessor에서는  
여러 프로세서와 여러 메모리 모듈을 서로 연결해야 한다는 뜻이다.

### 왜 processor끼리만 연결하면 안 되는가

shared memory 시스템에서는 각 프로세서가 계산만 하는 것이 아니라 다음 같은 일을 해야 한다.

- 메모리 읽기
- 메모리 쓰기
- 캐시 miss 처리
- coherence message 전달

그러므로 연결해야 하는 대상은 단순히 processor들끼리가 아니라,

- processor ↔ memory
- processor ↔ shared cache
- processor ↔ processor (간접적으로 coherence)
- cache ↔ memory

를 모두 포함하는 **전체 통신 구조**여야 한다.

### memory modules라고 한 이유

메모리는 하나의 큰 덩어리일 수도 있지만, 실제 하드웨어에서는 대역폭을 늘리기 위해 보통 여러 **memory module / bank / node**로 나뉜다.

즉 슬라이드는 단순히  
"CPU와 메모리를 선 하나로 연결"  
이 아니라,

**여러 CPU와 여러 memory module을 효율적으로 연결하는 구조 전체**

를 interconnection network라고 부르는 것이다.

> [!note]
> shared memory 시스템이라고 해도 실제 물리 구현에서는  
> 메모리가 여러 모듈로 나뉘는 경우가 많다.  
> 그래서 문제는 "CPU와 memory 연결"이 아니라  
> **많은 CPU와 많은 memory block을 어떻게 효율적으로 연결할 것인가**가 된다.

---

## 3. Types of interconnect

이 부분은 대표적인 interconnect topology를 나열한 것이다.

중요한 건 여기서 각각을 외우는 것보다,

**각 구조가 성능, 비용, 확장성 면에서 trade-off가 다르다**

는 점을 이해하는 것이다.

즉 topology를 볼 때는 항상 이런 질문을 해야 한다.

- 구현이 쉬운가?
- 동시에 많은 통신이 가능한가?
- 코어 수가 늘어나도 버틸 수 있는가?
- coherence 메시지를 잘 처리할 수 있는가?

---

## 4. Shared bus

가장 단순한 방식이다.

모든 프로세서와 메모리가 하나의 **공통 버스(shared bus)** 를 함께 쓴다.

그림으로 생각하면,

하나의 긴 통로가 있고 모든 CPU와 memory가 거기에 매달려 있는 느낌이다.

### 장점

- 구조가 단순하다
- 구현이 쉽다
- **snooping coherence와 잘 맞는다**

왜냐하면 버스에 올라온 요청을 모든 캐시가 동시에 볼 수 있기 때문이다.

### 단점

- 한 번에 사실상 제한된 수의 트랜잭션만 가능하다
- 프로세서 수가 늘어나면 버스가 병목이 된다
- 확장성이 낮다

### 언제 적합한가

- 작은 규모 SMP
- 코어 수가 적은 시스템
- 단순한 설계

즉 shared bus는 이해하기 쉬운 기본형이지만, 큰 시스템에는 잘 안 맞는다.

> [!example]
> bus에서는 `P0`가 메모리 요청을 보내면  
> 다른 모든 노드도 같은 bus를 본다.  
>  
> 그래서 snooping은 쉽지만,  
> 동시에 여러 요청이 몰리면 **모두가 같은 자원을 두고 경쟁**하게 된다.

> [!important]
> **shared bus = 단순 + snooping에 유리 + scalability 낮음**

---

## 5. Crossbar: Fully connected

crossbar는 각 입력이 각 출력으로 갈 수 있는 **전용 경로를 많이 제공하는 완전 연결형 구조**다.

### 직관

`N`개의 processor와 `M`개의 memory module이 있을 때,  
processor 하나가 memory 하나에 접근하는 경로가 비교적 독립적으로 마련되어 있다.

즉 shared bus처럼 모두가 한 줄을 두고 경쟁하는 게 아니라,  
서로 다른 쌍은 동시에 통신할 수 있다.

### 장점

- 병렬성이 높다
- 여러 요청이 동시에 다른 memory module로 갈 수 있다
- 지연이 비교적 작다

### 단점

- 하드웨어 비용이 매우 크다
- 연결 수가 많이 필요하다
- 시스템이 커질수록 복잡도가 급격히 증가한다

### 핵심 포인트

crossbar는 **성능은 좋지만 비싸다.**

즉

- shared bus = 싸고 단순하지만 병목
- crossbar = 빠르지만 비싸고 확장 어려움

이렇게 대비하면 된다.

> [!note]
> crossbar는 거의 "직접 연결에 가까운 풍부한 경로"를 제공하므로  
> contention을 줄이고 성능을 높일 수 있다.  
> 대신 그만큼 **배선과 스위치 비용**이 커진다.

> [!important]
> **crossbar = 낮은 hop/높은 병렬성 + 높은 비용**

---

## 6. Ring

ring은 노드들이 **원형으로 연결된 구조**다.

### 직관

각 노드는 양옆 노드와만 직접 연결되고, 데이터는 링을 따라 돈다.

### 장점

- bus보다 더 나은 확장성
- crossbar보다 구현이 단순
- 배선 비용이 상대적으로 낮음

### 단점

- 목적지까지 여러 hop을 거쳐야 할 수 있음
- 노드 수가 많아지면 평균 지연이 증가
- 트래픽이 몰리면 특정 구간이 병목이 될 수 있음

### 언제 자주 쓰이나

칩 내부(on-chip) interconnect에서 ring이 자주 등장한다.  
왜냐하면 배선이 단순하고 레이아웃이 비교적 쉽기 때문이다.

> [!example]
> ring에서 현재 노드의 반대편에 있는 목적지까지 가려면  
> 메시지가 중간 노드들을 여러 번 거쳐야 한다.  
>  
> 즉 **구조는 단순하지만 hop 수가 늘어날 수 있다.**

> [!important]
> **ring = 단순하고 실용적이지만, 노드 수가 커질수록 hop latency가 부담될 수 있음**

---

## 7. Mesh

mesh는 보통 **2차원 격자 형태**로 노드들을 연결하는 구조다.

### 직관

노드가 바둑판처럼 놓이고,  
각 노드는 위/아래/왼쪽/오른쪽 이웃과 연결된다.

### 장점

- 확장성이 좋다
- 큰 시스템 / 많은 코어에 적합하다
- 링크가 지역적으로 배치되어 배선이 현실적이다

### 단점

- 멀리 있는 노드까지 가려면 여러 hop 필요
- routing이 필요
- 트래픽 패턴에 따라 혼잡(congestion)이 생길 수 있음

### 왜 중요하냐

현대 many-core나 NoC(network-on-chip)에서 mesh는 아주 대표적인 구조다.

즉 규모가 커질수록  
shared bus나 crossbar보다 mesh류가 더 현실적이 된다.

> [!note]
> mesh는 "모든 노드를 다 직접 연결하지는 않지만",  
> **지역적 연결을 규칙적으로 반복**해서 큰 시스템을 만들기 좋다.

> [!important]
> **mesh = 현실적인 배선 + 좋은 확장성 + 여러 hop/routing 필요**

---

## 8. 2-D Torus

2-D torus는 mesh와 비슷하지만, **가장자리들이 서로 다시 연결되어 있는 구조**다.

### mesh와 차이

mesh에서는 가장자리 노드는 연결이 덜 되어 있는데,  
torus에서는 반대편 가장자리와 연결해서 **wrap-around 경로**를 만든다.

예를 들어:

- 맨 왼쪽 노드와 맨 오른쪽 노드가 연결
- 맨 위 노드와 맨 아래 노드가 연결

### 장점

- 평균 hop 수를 줄일 수 있음
- 링크 활용도가 좋아질 수 있음
- mesh보다 경로 선택이 더 유연함

### 단점

- 물리적인 배선이 더 복잡함
- 칩 위에서는 wrap-around 연결이 길어져 구현이 부담될 수 있음

> [!example]
> 일반 mesh에서는 왼쪽 끝에서 오른쪽 끝으로 가려면  
> 칸칸이 이동해야 한다.  
>  
> torus에서는 가장자리끼리 직접 이어져 있어서  
> **더 짧은 우회 경로**가 생길 수 있다.

> [!important]
> **2-D torus = mesh의 hop 문제를 완화하지만, 물리 배선은 더 어려워짐**

---

## 9. Hypercube

hypercube는 각 노드를 **비트 패턴**으로 보고,  
**한 비트만 다른 노드끼리 연결하는 구조**다.

### 직관

노드 수가 `2^k`일 때, 각 노드는 `k`개의 이웃과 연결된다.

예:

- 8노드면 3차원 cube
- 16노드면 4차원 hypercube

### 장점

- 노드 수가 많아져도 평균 hop 수가 비교적 작다
- 이론적으로 통신 효율이 좋다

### 단점

- 구조가 직관적이지 않고 구현이 복잡하다
- 물리 배선이 어렵다
- 현대 상용 칩의 물리적 레이아웃과는 덜 잘 맞는 경우가 많다

### 정리

hypercube는 고전적인 병렬 컴퓨터 topology로 중요하지만,  
실제 현대 칩에서는 mesh/ring 쪽이 더 자주 보인다.

> [!note]
> hypercube는 이론적으로 매우 예쁜 구조지만,  
> 실제 칩 설계에서는 **배선과 물리 배치의 현실성** 때문에 덜 선호될 수 있다.

---

## 10. Number of hops vs. number of links: Compare N processors and M memory modules

이 bullet이 이 슬라이드의 핵심 개념이다.

여기서 교수님이 말하려는 건:

interconnect를 비교할 때는 단순히 "연결되냐 안 되냐"가 아니라  
**몇 개의 링크가 필요한지**와  
**평균적으로 몇 hop을 가야 하는지**를 같이 봐야 한다는 것이다.

---

## 10.1 Number of links

링크 수는 **하드웨어 비용, 배선 복잡도, 면적, 전력**과 연결된다.

- 링크 많음 → 비용 큼
- 링크 적음 → 저렴함

예:

- shared bus: 링크 구조는 단순
- crossbar: 링크 엄청 많음
- mesh: 중간 정도
- hypercube: 규칙적이지만 차원 따라 증가

즉 링크 수는 **구현 비용 쪽 지표**다.

> [!important]
> **number of links = 비용 / 배선 / 면적 / 복잡도**

---

## 10.2 Number of hops

hop은 메시지가 목적지까지 가면서 거치는 **중간 단계 수**다.

예를 들어 ring에서 반대편 노드로 가면 여러 hop이 필요하다.

hop 수가 많아지면 보통:

- latency 증가
- congestion 가능성 증가
- coherence/message round-trip 시간 증가

즉 hop 수는 **성능 / 지연 쪽 지표**다.

> [!important]
> **number of hops = latency / 성능 / 메시지 전달 시간**

---

## 10.3 Trade-off

대부분 topology는 이 두 가지 사이에서 trade-off를 가진다.

### Shared bus

- 링크 수 적음
- hop 개념은 단순
- 하지만 contention 심함

### Crossbar

- hop 적음, 거의 직접 연결
- 하지만 링크 수 많고 비용 큼

### Ring

- 링크 수 적당
- hop 수는 노드 수에 따라 증가

### Mesh

- 링크 수는 crossbar보다 훨씬 적고 현실적
- hop 수는 crossbar보다 큼
- 대신 scalability 좋음

즉 결국 질문은 이거다:

**더 비싼 하드웨어를 써서 짧은 경로를 만들 것인가,  
아니면 싼 하드웨어를 써서 hop을 조금 더 감수할 것인가?**

> [!summary]
> interconnect 설계의 핵심 trade-off:
>
> - **많은 링크** → 빠를 수 있지만 비쌈
> - **적은 링크** → 싸지만 hop이나 contention이 늘 수 있음

---

## 11. 왜 coherence와 연결되냐

이 슬라이드가 단순 네트워크 슬라이드가 아니라  
**cache coherence 강의 안에 들어간 이유**가 중요하다.

coherence protocol은 단지 상태만 바꾸는 논리가 아니고,  
실제로는 **메시지를 네트워크 위로 보내야 한다.**

예:

- invalidate
- read miss request
- write miss request
- fetch
- write-back
- data reply

이 모든 것이 interconnect를 통해 이동한다.

따라서 interconnect가 다르면:

- coherence latency도 달라지고
- scalability도 달라지고
- snooping이 가능한지도 달라진다

예를 들어:

- shared bus는 snooping과 잘 맞음
- distributed network는 directory가 더 적합함

즉 interconnect topology는 **coherence protocol 선택과 직접 연결된다.**

> [!danger]
> coherence는 추상적인 상태 머신만의 문제가 아니다.  
> 실제 하드웨어에서는  
> **그 상태 변화를 실어 나를 interconnect가 반드시 필요하다.**  
>  
> interconnect가 달라지면 coherence 구현 방식 자체도 달라질 수 있다.

---

## 12. 앞 슬라이드와 겹치는 부분 / 새로 추가된 부분

### 이미 앞 슬라이드에서 본 내용

- shared-memory MP에는 interconnect가 필요하다
- UMA / NUMA와 연결 구조는 중요하다

### 이 슬라이드에서 새로 추가된 것

- interconnect의 구체적인 종류들
- topology를 비교하는 기준
- **links vs hops**라는 설계 trade-off

즉 이 슬라이드는 완전 새 주제라기보다는  
앞 슬라이드의 **Need interconnection network**를 더 세부적으로 푼 슬라이드다.

---

## 13. 시험/이해용 핵심 요약

### 한 줄 요약

shared-memory multiprocessor에서는 processor와 memory module을 연결하는 interconnect가 필요하며, interconnect topology는 **비용, 지연, 대역폭, 확장성**을 결정한다.

### 구조별 아주 짧은 요약

- **Shared bus**: 가장 단순, snooping에 유리, 확장성 낮음
- **Crossbar**: 성능 좋음, 완전 연결, 비용 큼
- **Ring**: 단순하고 실용적, hop 증가 가능
- **Mesh**: 확장성 좋음, many-core에 적합
- **2-D Torus**: mesh 개선형, 평균 hop 감소
- **Hypercube**: 이론적으로 효율적, 구현은 복잡

### 비교 기준

- **Number of links** → 비용 / 복잡도
- **Number of hops** → 지연 / 성능

> [!tip]
> 시험에서 topology 비교를 물으면  
> 그냥 이름만 나열하지 말고 반드시
>
> - **구현 비용**
> - **평균 hop**
> - **확장성**
> - **coherence와의 궁합**
>
> 을 같이 말하면 좋다.

---

## 14. 다음 내용과의 연결

이 슬라이드 다음에는 보통 이런 방향으로 연결된다.

어떤 interconnect를 쓰느냐에 따라

- snooping이 가능한지
- broadcast가 감당 가능한지
- directory가 필요한지
- NUMA가 왜 필요한지

같은 문제가 달라진다.

즉 이 슬라이드는 단순히 네트워크 종류를 외우는 슬라이드가 아니라,

**cache coherence가 실제 하드웨어 위에서 어떻게 구현될 수 있는지 이해하기 위한 기반 슬라이드**

라고 보면 된다.

> [!summary]
> 한 줄로 끝내면:
>
> **Interconnect는 processor와 memory를 연결하는 통신 구조이며, topology에 따라 coherence의 성능, 비용, 확장성이 달라진다.**


# Shared Memory Multiprocessors: Memory Hierarchy

> [!summary]
> 이 슬라이드는 shared-memory multiprocessor에서 **왜 memory hierarchy, 특히 cache hierarchy가 필요한지**를 설명하는 슬라이드다.  
> 여러 프로세서가 동시에 메모리를 공유하면 memory traffic과 bandwidth 요구가 폭증하므로, 이를 줄이기 위해 **다단계 캐시 계층 구조**가 필요하다.  
> 그리고 바로 이 캐시들이 **나중에 cache coherence 문제를 만들어내는 직접적인 원인**이 된다.

---

## 1. 이 슬라이드의 역할

이 슬라이드는 shared-memory multiprocessor에서 왜 **memory hierarchy**, 특히 **cache hierarchy**가 필요한지를 설명하는 슬라이드다.

앞 슬라이드까지의 흐름은 이랬다.

- 여러 프로세서가 같은 메모리를 공유한다.
- 그러려면 interconnect가 필요하다.
- 그런데 여러 프로세서가 동시에 메모리를 요청하면 memory system에 엄청난 부담이 걸린다.
- 그래서 그 부담을 줄이기 위해 **캐시(cache)** 가 필요하다.

즉 이 슬라이드는

**왜 shared-memory multiprocessor에서는 캐시가 필수인가?**

를 설명하는 배경 슬라이드다.

> [!important]
> 이 슬라이드의 핵심은  
> **캐시는 단순히 빠른 저장소가 아니라, shared memory 시스템이 감당 가능하도록 만들어주는 필수 구조**라는 점이다.

---

## 2. Problem: sharing memory means more than one processor can send requests to memory

이 bullet은 shared memory의 가장 직접적인 하드웨어 문제를 말한다.

의미:

메모리를 공유한다는 것은, 여러 프로세서가 동시에 같은 메모리 시스템에 요청을 보낼 수 있다는 뜻이다.

단일 프로세서 시스템에서는 보통 한 CPU만 memory request를 보낸다.  
하지만 multiprocessor에서는 다음처럼 될 수 있다.

- `P0`가 load 요청
- `P1`가 store 요청
- `P2`가 instruction fetch miss
- `P3`가 data read miss

이런 요청들이 같은 시점에 몰릴 수 있다.

### 왜 문제인가

메모리는 무한히 빠르지 않다.  
따라서 여러 프로세서가 동시에 접근하면:

- 메모리 컨트롤러가 바빠짐
- 대역폭이 부족해짐
- 지연(latency)이 증가함
- 병목(bottleneck)이 생김

즉 shared memory의 장점은 크지만,  
하드웨어 관점에서는 **memory traffic explosion** 문제가 생긴다.

---

## 2.1 High memory bandwidth required

이 서브 bullet은 위 문제의 직접적인 결과다.

의미:

여러 프로세서가 동시에 메모리를 때리므로, 시스템은 매우 높은 **memory bandwidth**를 필요로 한다.

여기서 memory bandwidth는  
**단위 시간당 메모리가 처리할 수 있는 데이터 양**이다.

예를 들어:

- CPU 하나는 초당 `10GB/s` 정도를 원할 수 있고
- CPU가 8개면 훨씬 더 많은 총 bandwidth가 필요할 수 있다

즉 processor 수가 늘수록 메모리 대역폭 요구도 커진다.

### 중요한 직관

프로세서가 빨라질수록, 코어가 많아질수록, memory bandwidth 문제는 더 심해진다.

그래서 shared-memory multiprocessor 설계에서는 항상 질문이 생긴다.

**"모든 요청을 DRAM까지 보내면 감당이 가능한가?"**

보통 답은 **아니오**이고, 그래서 캐시를 둔다.

> [!warning]
> shared memory에서는 모든 코어가 같은 memory system을 두드릴 수 있기 때문에  
> 문제는 단순한 latency만이 아니라  
> **총 bandwidth demand 자체가 폭증한다는 것**이다.

---

## 3. To avoid sending lots of memory requests, processors use caches to:

이 bullet은 바로 위 문제의 해결책을 말한다.

즉:

메모리로 가는 요청 수를 줄이기 위해 프로세서들은 **캐시**를 사용한다.

여기서 캐시는 단순히 "조금 빠른 저장소"가 아니라,

- 요청을 걸러내고
- 평균 접근 시간을 줄이고
- 메모리 대역폭 소비를 줄이는

핵심 장치다.

---

## 3.1 Filter out many memory requests

이게 캐시의 가장 기본적인 역할이다.

의미:

메모리까지 갈 필요가 없는 요청들을 캐시가 중간에서 처리한다.

예를 들어 어떤 프로세서가 같은 변수 `A`를 계속 읽는다면,  
매번 DRAM에 갈 필요 없이 `L1/L2/L3` 중 하나에서 해결하면 된다.

즉 캐시는 메모리 입장에서 보면 **request filter** 역할을 한다.

### 왜 중요한가

캐시가 없다면:

- 모든 load/store가 메모리 시스템으로 향함
- shared memory multiprocessor에서는 메모리 병목이 매우 빨리 발생함

캐시가 있으면:

- 자주 쓰는 데이터는 로컬에 남음
- 메모리 시스템은 진짜 필요한 miss만 처리하면 됨

즉 캐시는 **traffic reduction device**라고도 볼 수 있다.

> [!note]
> 캐시의 첫 번째 역할은  
> "CPU를 빠르게 해주는 것" 이전에  
> **메모리까지 가는 요청 자체를 줄여주는 것**이다.

---

## 3.2 Reduce average memory latency

캐시는 평균 메모리 접근 시간을 줄인다.

여기서 핵심은 **average**라는 단어다.

DRAM 접근은 느리지만,  
많은 접근이 캐시에서 hit 나면 평균 접근 시간은 훨씬 낮아진다.

예를 들어:

- `L1 hit`: 매우 빠름
- `L2 hit`: 조금 느림
- `L3 hit`: 더 느림
- `DRAM miss`: 매우 느림

프로그램의 많은 접근이 `L1/L2`에서 해결되면,  
전체 평균 지연은 DRAM만 썼을 때보다 크게 낮아진다.

### 왜 multiprocessor에서 더 중요하냐

shared memory multiprocessor에서는 DRAM까지 가는 길이 더 복잡할 수 있다.

- interconnect를 건너야 할 수도 있고
- shared cache를 지나야 할 수도 있고
- remote memory access일 수도 있다

그래서 캐시 hit의 가치는 단일 프로세서 때보다 더 커질 수 있다.

> [!important]
> multiprocessor에서는 DRAM access가 단순히 "느린 메모리 접근"이 아니라  
> **공유 자원 사용 + interconnect 사용 + bandwidth 소비**까지 동반할 수 있다.  
> 그래서 cache hit의 가치가 더 크다.

---

## 3.3 Reduce memory bandwidth requirements

이건 3.1과 비슷하지만, 조금 다른 관점이다.

- 3.1은 요청 수 자체를 줄인다는 의미
- 3.3은 메모리 시스템이 실제로 처리해야 할 **bandwidth 부담**을 줄인다는 의미

즉 캐시가 있으면 DRAM 쪽으로 나가는 트래픽이 감소한다.

예:

- 같은 cache line을 여러 번 재사용하면 DRAM은 한 번만 공급
- write-back 정책이면 매 write마다 DRAM까지 안 감
- locality가 좋으면 많은 요청이 캐시 내부에서 해결됨

결국 메모리는 더 적은 데이터 이동만 감당하면 된다.

### shared memory와의 연결

멀티프로세서에서는 processor 수가 많아져서 bandwidth 요구가 커지므로,  
캐시는 단순한 성능 향상 장치가 아니라  
**시스템 확장성을 가능하게 하는 필수 장치**다.

> [!important]
> **캐시가 없으면 shared-memory multiprocessor는 memory bandwidth 때문에 쉽게 무너진다.**  
> 즉 캐시는 luxury가 아니라 necessity다.

---

## 4. Typically more than one level of caches is used

이 bullet은 현대 시스템이 왜 **다단계 캐시 계층 구조(cache hierarchy)** 를 쓰는지를 말한다.

의미:

보통 캐시는 한 단계만 두지 않고, 여러 레벨(`L1`, `L2`, `L3`)로 구성한다.

### 왜 여러 단계가 필요하냐

한 가지 캐시만으로는 두 가지 목표를 동시에 만족시키기 어렵다.

- 아주 빠르게 만들고 싶다
- 아주 크게 만들고 싶다

하지만 일반적으로:

- 작은 캐시는 빠르게 만들기 쉽다
- 큰 캐시는 느려지기 쉽다

그래서 설계자는 보통 여러 단계를 둔다.

- 맨 위는 작고 빠름
- 아래로 갈수록 크고 느림

즉 cache hierarchy는  
**speed vs. capacity trade-off**를 계층적으로 해결하는 방법이다.

> [!note]
> 한 줄로 말하면:
>
> **작고 빠른 캐시와 크고 느린 캐시를 층층이 쌓아서, 속도와 용량을 동시에 얻으려는 구조**가 cache hierarchy다.

---

## 5. L1 caches: Usually Split I & D caches, small and fast

이 bullet은 `L1` 캐시의 특징을 말한다.

### Split I & D caches

`L1`은 보통 **Instruction cache**와 **Data cache**로 나뉜다.

즉:

- `L1 I-cache`: 명령어 저장
- `L1 D-cache`: 데이터 저장

### 왜 split하냐

명령어 접근과 데이터 접근을 분리하면 다음 장점이 있다.

- 동시에 instruction fetch와 data access를 처리하기 쉬움
- 구조가 단순해짐
- 성능 향상 가능

예를 들어 CPU가 한 cycle에

- 명령어 하나 fetch
- 데이터 하나 load

를 하려면 I-cache와 D-cache가 나뉘어 있는 게 유리하다.

### Small and fast

`L1`은 가장 CPU에 가깝기 때문에 아주 빠르면서도 작다.

왜 작아야 하냐면:

- 큰 캐시는 access time이 길어짐
- CPU pipeline의 아주 초기 단계에서 hit 여부를 알아야 함
- `L1`이 느리면 전체 CPU clock / pipeline에 큰 영향을 줌

그래서 `L1`은 용량보다 속도를 더 우선시한다.

### 핵심 직관

`L1`은

**"가장 자주 쓰는 것만 담는 초고속 캐시"**

라고 생각하면 된다.

> [!important]
> **L1 = 가장 작고 가장 빠른 캐시**  
> CPU에 가장 가깝기 때문에 latency가 극도로 중요하다.

---

## 6. L2 caches: Usually on die, composed of SRAM cells

이 bullet은 `L2`의 특징을 설명한다.

### Usually on die

`L2`는 보통 칩 안(**on-die**)에 있다.

즉 프로세서와 같은 실리콘 다이 위에 구현된다.

왜냐하면:

- off-die보다 훨씬 빠르고
- interconnect 지연이 적고
- bandwidth 확보가 유리하기 때문이다

### Composed of SRAM cells

`L2`는 보통 **SRAM**으로 만들어진다.

SRAM은:

- 빠르다
- 면적을 많이 먹는다
- DRAM보다 비싸다

그래서 캐시는 대체로 SRAM 기반이고,  
메인 메모리는 DRAM 기반이다.

### L2의 역할

`L2`는 `L1`보다:

- 더 크고
- 더 느리지만
- still much faster than DRAM

즉 `L1 miss`를 많이 흡수해서 DRAM까지 가는 빈도를 줄인다.

> [!note]
> `L2`는 보통  
> **L1이 놓친 것을 많이 받아주는 두 번째 방어선** 역할을 한다.

---

## 7. L3 caches: On-die or off-die, SRAM or eDRAM cells

이 bullet은 `L3`의 특징을 말한다.

### On-die or off-die

`L3`는 설계에 따라:

- 칩 내부(on-die)에 있을 수도 있고
- 칩 외부(off-die)에 있을 수도 있다

현대 CPU에선 on-die shared `L3`가 흔하지만,  
역사적으로나 특정 구조에선 off-die도 가능하다.

### SRAM or eDRAM cells

`L3`는 더 큰 용량이 필요하므로 SRAM만 쓰면 면적/비용 부담이 커질 수 있다.  
그래서 경우에 따라 **eDRAM**을 쓸 수도 있다.

- SRAM: 빠르지만 면적 큼
- eDRAM: 더 조밀하지만 상대적으로 복잡/느릴 수 있음

### L3의 역할

`L3`는 보통:

- 여러 코어가 공유하는 cache
- `L2 miss`를 줄여주는 마지막 on-chip cache
- coherence에서도 중요한 역할

을 한다.

특히 shared-memory multiprocessor에서는 `L3`가 단순한 캐시가 아니라,  
**코어 간 데이터 공유와 coherence의 중심점**이 되기도 한다.

> [!important]
> `L3`는 종종  
> **마지막 큰 on-chip buffer이자, 여러 코어가 만나는 공유 캐시 계층**으로 동작한다.

---

## 8. 이 슬라이드가 cache coherence와 어떻게 연결되냐

이 슬라이드는 아직 coherence protocol을 직접 설명하는 건 아니지만,  
coherence를 왜 해야 하는지와 아주 깊게 연결되어 있다.

### 핵심 연결고리

shared-memory multiprocessor는 성능 때문에 캐시를 반드시 써야 한다.

그런데 여러 프로세서가 각자 캐시를 가지면  
같은 데이터가 여러 캐시에 동시에 복사될 수 있다.

예:

- `P0 L1`에 `X`
- `P1 L1`에 `X`
- `P2 L2`나 `L3`에도 관련 line

이제 `P0`가 `X`를 바꾸면,  
다른 캐시의 `X`는 어떻게 처리할 것인가?

이게 바로 **coherence 문제**다.

즉:

- shared memory → memory traffic 많음
- traffic 줄이려면 cache hierarchy 필요
- cache hierarchy를 쓰면 same data copies가 여러 군데 생김
- 그래서 cache coherence 필요

이 슬라이드는 그 **2번과 3번 사이를 설명하는 다리 역할**을 한다.

> [!danger]
> 캐시는 문제를 해결하면서 동시에 새로운 문제를 만든다.
>
> - 해결하는 문제: latency / bandwidth / traffic
> - 새로 만드는 문제: **같은 데이터의 여러 사본 관리**, 즉 coherence

---

## 9. 이 슬라이드의 핵심 논리 흐름

이 슬라이드를 문장으로 다시 쓰면 대충 이렇게 된다.

shared memory에서는 여러 프로세서가 동시에 메모리 요청을 보낼 수 있다.

따라서 메모리 bandwidth 요구가 매우 커진다.

이를 줄이기 위해 캐시를 사용한다.

캐시는 memory request를 걸러내고, 평균 지연을 줄이고, bandwidth 요구를 줄인다.

현대 시스템은 보통 하나가 아니라 여러 단계의 cache hierarchy를 사용한다.

각 레벨은 속도와 크기에서 서로 다른 trade-off를 가진다.

> [!summary]
> 이 슬라이드의 전체 흐름:
>
> 1. shared memory에서는 request가 많이 몰린다  
> 2. 그래서 bandwidth pressure가 생긴다  
> 3. 이를 줄이려면 cache가 필요하다  
> 4. cache는 여러 단계로 구성된다  
> 5. 그런데 여러 cache에 같은 data가 생기므로 coherence가 필요해진다

---

## 10. 단일 프로세서와 비교해서 왜 더 중요하냐

단일 프로세서에서도 캐시는 중요하다.  
하지만 멀티프로세서에서는 훨씬 더 절박하다.

### 단일 프로세서

캐시는 주로:

- latency hiding
- locality exploitation

용도다.

### Shared-memory multiprocessor

그것뿐 아니라 다음도 해야 한다.

- 많은 코어가 동시에 DRAM을 때리는 것을 막아야 함
- bandwidth crisis를 완화해야 함
- coherence도 관리해야 함

즉 멀티프로세서에서 캐시는 단순 가속 장치가 아니라  
**시스템이 아예 성립하기 위해 필요한 기본 구조**다.

> [!important]
> 단일 코어에서는 cache가 "매우 중요"하다면,  
> 멀티코어 shared memory에서는 cache가 **없으면 시스템이 버티기 어려운 수준으로 필수적**이다.

---

## 11. 시험에서 잡아야 할 포인트

### 포인트 1

shared memory에서는 여러 프로세서가 동시에 메모리를 요청할 수 있기 때문에  
**bandwidth pressure**가 매우 크다.

### 포인트 2

캐시의 역할은 단순히 "빠른 저장소"가 아니라:

- memory requests filtering
- average latency reduction
- bandwidth demand reduction

이다.

### 포인트 3

현대 프로세서는 보통 **다단계 cache hierarchy**를 사용한다.

### 포인트 4

`L1`은 작고 빠르며 보통 instruction/data로 분리된다.

### 포인트 5

`L2`와 `L3`는 더 크고 느리지만, 메모리 트래픽을 크게 줄여준다.

### 포인트 6

이렇게 캐시를 여러 프로세서가 각자 가지게 되면  
**coherence 문제가 생긴다.**

> [!tip]
> 시험에서는
>
> **"왜 캐시가 필요한가?"** 와  
> **"왜 캐시가 coherence 문제를 만드는가?"**
>
> 를 연결해서 말할 수 있어야 한다.

---

## 12. 아주 짧은 한 줄 정리

### 한 줄 요약

shared-memory multiprocessor에서는 많은 memory request와 높은 bandwidth 요구를 감당하기 위해 **다단계 cache hierarchy**가 필요하며, 이것이 이후 **cache coherence 문제의 직접적인 원인**이 된다.

---

## 13. 앞 슬라이드와의 연결

### 앞 슬라이드

- processor와 memory를 연결하는 interconnect가 필요하다

### 이번 슬라이드

- 그런데 단순히 연결만으로는 부족하고
- memory traffic 자체가 너무 많으므로
- cache hierarchy가 필요하다

즉 흐름은 이렇게 이어진다.

- shared memory
- interconnect 필요
- memory traffic 많음
- caches 필요
- caches 때문에 coherence 필요

> [!summary]
> 이 장의 흐름은 보통 이렇게 이어진다:
>
> **shared memory → interconnect → bandwidth pressure → cache hierarchy → coherence**

---

## 14. 예시로 다시 보기

### 예시 1: 캐시가 없을 때

4개의 프로세서가 모두 배열을 반복적으로 읽는다고 하자.

그러면 매 접근마다 DRAM까지 가야 해서:

- latency 큼
- bandwidth 요구 큼
- interconnect 붐빔

### 예시 2: 캐시가 있을 때

처음 한 번만 line을 가져오고, 이후 반복 접근은 `L1/L2/L3`에서 처리된다.

그러면:

- 평균 접근 시간 감소
- DRAM 요청 감소
- shared memory system의 부담 감소

### 예시 3: coherence 문제 등장

`P0`와 `P1`이 같은 `counter` 값을 각자 `L1`에 들고 있다가,  
`P0`가 값을 바꾸면 `P1`의 copy는 **stale**해질 수 있다.

그래서 cache hierarchy가 문제를 해결하는 동시에  
새로운 문제(**coherence**)도 만든다.

> [!example]
> 즉 캐시는:
>
> - **성능 문제를 해결해주는 도구**
> - 동시에 **일관성 문제를 만드는 원인**
>
> 이라는 두 얼굴을 가진다.


# Cache Coherence

> [!summary]
> 이 슬라이드 묶음은 이제 본격적으로 **cache coherence가 왜 필요한지**, **무엇을 보장해야 하는지**, 그리고 **어떤 방식으로 구현하는지**를 설명하기 시작하는 부분이다.  
> 핵심은 shared-memory multiprocessor에서 **같은 데이터의 여러 캐시 복사본**이 생길 때, 이 복사본들이 서로 모순되지 않도록 만드는 것이다.

---

## 1. 이 슬라이드 묶음의 전체 역할

이 슬라이드들은 이제 본격적으로 **cache coherence가 왜 필요한지**,  
**무엇을 만족해야 하는지**,  
**어떤 방식으로 구현하는지**를 설명하기 시작하는 부분이다.

앞 슬라이드까지의 흐름은 이랬다.

- shared-memory multiprocessor에서는 여러 프로세서가 같은 메모리를 공유한다.
- 여러 프로세서가 동시에 메모리를 요청하면 bandwidth pressure가 매우 커진다.
- 그래서 cache hierarchy가 필요하다.
- 그런데 캐시를 쓰면 같은 메모리 위치의 복사본이 여러 캐시에 동시에 존재할 수 있다.
- 이때 한 프로세서가 값을 바꾸면 다른 캐시의 값이 stale해질 수 있다.
- 따라서 **cache coherence protocol**이 필요하다.

즉 이 슬라이드 묶음은

**캐시를 여러 개 쓰는 shared-memory 시스템에서, "같은 데이터의 여러 복사본" 문제를 어떻게 다룰 것인가?**

를 설명하는 핵심 부분이다.

> [!important]
> 이제부터는 단순히 "캐시가 왜 필요한가?"가 아니라  
> **"여러 캐시가 동시에 존재할 때 correctness를 어떻게 보장할 것인가?"** 가 핵심 질문이 된다.

---

## 2. Cache Coherence

---

## 2.1 문제의 핵심

### Problem: Using caches means multiple copies of the same memory location may exist

이 문장은 coherence 문제의 출발점이다.

의미:

캐시를 사용하면 **같은 메모리 위치가 여러 캐시에 동시에 복사될 수 있다.**

예를 들어 메모리 주소 `A`가 있다고 하자.

- Processor 1 cache에도 `A`의 복사본이 있음
- Processor 2 cache에도 `A`의 복사본이 있음

이건 아주 자연스러운 상황이다.  
왜냐하면 둘 다 `A`를 읽었을 수 있기 때문이다.

문제는 그 다음이다.

- `P1`이 `A`를 수정하면
- `P2` cache 안에 남아 있는 `A` 값은 이제 오래된 값(**stale data**)이 될 수 있다.

즉 여러 캐시의 복사본이 **서로 다른 값**을 가지게 될 수 있다.

### Updates to the same location may lead to bugs

같은 위치를 여러 프로세서가 공유하는데, 그 중 하나가 값을 바꾸었을 때 다른 쪽이 그걸 못 보면 프로그램이 잘못 동작할 수 있다.

이건 단순한 성능 문제가 아니라 **정확성(correctness) 문제**다.

예를 들어:

    flag = 1;

를 `P1`이 썼는데, `P2`가 여전히 `flag == 0`이라고 본다면  
동기화 자체가 깨질 수 있다.

또는

    counter = counter + 1;

같은 공유 데이터 업데이트가 제대로 보이지 않으면  
병렬 프로그램의 결과가 틀어질 수 있다.

즉 cache coherence는 **"있으면 좋은 최적화"** 가 아니라,  
공유 메모리 모델이 올바르게 동작하기 위해 반드시 필요한 하드웨어 기능이다.

> [!danger]
> coherence 문제는 성능 문제가 아니라 **correctness 문제**다.  
> stale copy를 허용하면 shared memory 프로그래밍 모델 자체가 깨질 수 있다.

---

## 2.2 슬라이드의 예시

슬라이드의 예시는 아주 단순하다.

- Processor 1 reads `A`
- Processor 2 reads `A`
- Processor 1 writes to `A`

이 과정을 단계별로 보면:

### Step 1: Processor 1 reads A

`P1`이 `A`를 읽어서 자기 캐시에 가져온다.

### Step 2: Processor 2 reads A

`P2`도 `A`를 읽어서 자기 캐시에 가져온다.

이 시점에서는:

- `P1` cache에도 `A`
- `P2` cache에도 `A`

둘 다 같은 값을 갖고 있으므로 아직 문제는 없다.

### Step 3: Processor 1 writes to A

`P1`이 `A`를 수정한다.

이제:

- `P1` cache에는 새 값
- `P2` cache에는 옛날 값

이 될 수 있다.

즉

**Now, processor 2’s cache contains stale data**

가 된다.

> [!example]
> 읽기만 할 때는 여러 캐시에 같은 copy가 있어도 문제가 없다.  
> 문제는 **누군가 write하는 순간** 시작된다.

---

## 2.3 결론

### Cache coherence need to be implemented in hardware using a cache coherence protocol

뜻:

이런 문제는 하드웨어 차원에서 **cache coherence protocol**을 구현해서 해결해야 한다.

즉 프로그래머가 매번 직접  
"저 캐시들 좀 업데이트해줘"  
라고 할 수는 없고, 하드웨어가 자동으로:

- invalidate 하거나
- update 하거나
- ownership을 관리하거나
- 최신 값을 전달하는

프로토콜을 수행해야 한다.

> [!important]
> coherence는 소프트웨어가 매번 수동으로 처리하는 것이 아니라,  
> **하드웨어 프로토콜**이 자동으로 유지해야 한다.

---

## 3. Conditions for Cache Coherence

이 슬라이드는 무엇을 만족하면 coherence가 되는가를 정리한 슬라이드다.

즉 **"coherence protocol은 정확히 어떤 성질을 보장해야 하는가?"** 를 말한다.

세 가지 핵심 조건이 있다.

- Program Order
- Coherent View of Memory
- Write Serialization

---

## 3.1 Program Order

슬라이드 문장:

> A read by processor P to location A that follows a write by P to A, with no writes to A by another processor in between, should always return the value of A written by P

이 말은 같은 프로세서 입장에서 아주 상식적인 요구다.

### 뜻

어떤 프로세서 `P`가 자기 자신이 방금 `A`에 쓴 값을,  
그 다음에 다시 `A`를 읽었을 때,  
다른 프로세서가 그 사이에 `A`를 바꾸지 않았다면  
반드시 자기가 쓴 값을 읽어야 한다.

예시:

    A = 10;
    x = A;

이 코드에서 같은 프로세서가 `A = 10` 한 직후 `A`를 다시 읽었는데  
`x`가 `10`이 아니라 옛날 값이면 말이 안 된다.

즉:

**내 write 다음에 내 read가 있으면, 중간에 다른 사람이 안 건드린 한 내가 쓴 값이 보여야 한다.**

이건 아주 최소한의 올바름 조건이다.

### 왜 coherence에서 이것을 언급하냐

캐시 구조와 write buffering 같은 최적화 때문에  
하드웨어 입장에서는 값이 여러 곳에 있을 수 있다.

그렇더라도 적어도 같은 프로세서 자신에게는  
자기 write가 반영된 것처럼 보여야 한다.

이 조건은 **same-processor self-consistency** 같은 기본 기대를 표현한다.

> [!note]
> Program Order는  
> **"적어도 나는 내가 방금 쓴 값을 다시 읽을 수 있어야 한다"**  
> 는 최소한의 상식을 보장한다.

---

## 3.2 Coherent View of Memory

슬라이드 문장:

> A read by processor P1 to location A that follows a write by another processor P2 to location A should return the written value by P2 if:
>
> - The read and write are sufficiently separated in time
> - No other writes to A by another processor occur between the read and the write

이건 다른 프로세서가 쓴 값을 내가 읽을 때의 조건이다.

### 뜻

`P2`가 `A`에 쓴 뒤 충분한 시간이 지났고,  
중간에 다른 누군가가 `A`를 다시 쓰지 않았다면,  
`P1`이 `A`를 읽을 때 `P2`가 쓴 최신 값을 봐야 한다.

### 왜 "sufficiently separated in time"이 들어가나

하드웨어는 실제로 순간이동처럼 동시에 모든 캐시를 바꾸지 못한다.

예를 들어:

- 한 프로세서가 write
- invalidate 메시지 전달
- 다른 캐시가 자기 copy 무효화
- 새 값 fetch

이런 과정에는 시간이 든다.

그래서 coherence는 보통  
**"즉시 전 우주에 동시에 반영"** 을 요구하는 게 아니라,

**적절한 시간이 지난 뒤에는 모든 프로세서가 일관된 최신 값을 봐야 한다**

는 식으로 표현된다.

예시:

`P2`가

    A = 99;

를 했고, 그 뒤 `P1`이 `A`를 읽었는데  
중간에 다른 누군가가 `A`를 다시 안 바꿨다면,  
`P1`은 결국 `99`를 봐야 한다.

이게 **coherent view of memory**다.

### 핵심 직관

같은 메모리 위치 `A`에 대해서는  
프로세서들이 완전히 제각각의 세계를 보면 안 된다.

즉 시간이 충분히 지나면,  
모두가 **같은 최신 값을 향해 수렴**해야 한다.

> [!important]
> coherence는 "즉시 동시 반영"까지는 요구하지 않더라도,  
> **충분한 시간이 지나면 모두가 같은 최신 값을 봐야 한다**는 것을 요구한다.

---

## 3.3 Write Serialization

슬라이드 문장:

> Writes to the same location are serialized: Two writes to the same location by any two processors are seen in the same order by all processors

이건 coherence 조건 중에서도 가장 중요하다.

### 뜻

같은 메모리 위치 `A`에 대해 여러 프로세서가 write를 하면,  
그 write들은 어떤 하나의 **전역적인 순서**로 정렬되어야 하고,  
모든 프로세서가 그 순서를 같게 봐야 한다.

즉 write들이 뒤죽박죽 다른 순서로 보이면 안 된다.

예시:

- `P1` writes `A = 1`
- `P2` writes `A = 2`

이 두 write가 일어났다고 하자.

그러면 모든 프로세서는 이 둘을 같은 순서로 봐야 한다.

예를 들어 시스템이  
"먼저 `A = 1`, 그 다음 `A = 2`"  
라고 정했다면

- `P3`도 그 순서로 봐야 하고
- `P4`도 그 순서로 봐야 한다.

어떤 프로세서는 `2`가 먼저, 어떤 프로세서는 `1`이 먼저처럼 보면 안 된다.

### 왜 중요한가

같은 주소에 대한 write 순서가 프로세서마다 다르게 보이면  
메모리에 대한 공통된 현실(shared reality)이 사라진다.

coherence는 적어도 **한 주소에 대해서는**  
모두가 같은 write history를 보게 해야 한다.

> [!danger]
> 같은 주소에 대한 write 순서를 프로세서마다 다르게 보면  
> shared memory는 더 이상 "공유된 하나의 값"처럼 동작하지 않는다.

---

## 3.4 이 세 조건의 의미 정리

### Program Order

같은 프로세서는 자기 write 뒤의 자기 read에서 자기 값을 봐야 한다.

### Coherent View of Memory

한 프로세서가 쓴 값은 시간이 지나면 다른 프로세서도 볼 수 있어야 한다.

### Write Serialization

같은 주소에 대한 여러 write는 모든 프로세서에게 같은 순서로 보여야 한다.

---

## 3.5 매우 중요한 구분

이 슬라이드는 **cache coherence의 조건**을 말하는 것이다.  
즉 **한 memory location에 대한 조건**이다.

여기서 아직 말하고 있는 것은:

- 주소 `A` 하나에 대한 동작
- 같은 location에 대한 read/write

이지, 여러 주소 간의 순서 전체는 아니다.

그건 다음 슬라이드의 **memory consistency models**와 비교해서 나온다.

> [!important]
> **Coherence = one location**  
> **Consistency = multiple locations ordering**
>
> 이 구분은 시험에서 매우 자주 중요하다.

---

## 4. Cache Coherence Protocol Classification

이 슬라이드는 coherence와 consistency를 구분하고,  
coherence protocol의 큰 분류를 소개한다.

---

## 4.1 Cache coherence defines behavior of reads and writes to the same memory location

이 문장은 coherence의 범위를 정확히 정의한다.

뜻:

cache coherence는 **같은 메모리 위치**에 대한 read/write의 동작을 정의한다.

즉 coherence는 주소 `A`에 대해:

- 누가 썼는지
- 누가 최신 값을 봐야 하는지
- write 순서가 어떻게 보여야 하는지

를 말한다.

---

## 4.2 Compared to: Memory consistency models define the behavior of reads and writes with respect to accesses to other memory locations

이 문장은 coherence와 consistency의 차이를 말한다.

### coherence

같은 주소에 대한 규칙

예:

- `A`에 대한 두 write는 어떤 순서인가?
- `A`를 읽으면 어떤 값을 봐야 하나?

### consistency

서로 다른 주소들 사이의 순서에 대한 규칙

예:

    A = 1;
    flag = 1;

이 순서가 다른 프로세서에게도 꼭 그 순서로 보여야 하나?

즉 `A`와 `B` 같은 **다른 주소들 사이의 ordering**을 어떻게 해석할 것인가를 다룬다.

### 아주 쉽게 구분

- **Coherence = one location problem**
- **Consistency = many locations ordering problem**

즉 coherence가 먼저 해결해야 하는 더 기본 문제고,  
consistency는 그 위에 얹히는 더 넓은 의미의 메모리 모델 문제다.

> [!note]
> coherence는  
> **"주소 A 하나에 대해 모두가 같은 현실을 보게 할 것인가?"**
>
> consistency는  
> **"주소 A와 B 사이의 순서를 어떻게 보게 할 것인가?"**
>
> 를 다룬다.

---

## 4.3 Two main types of cache coherence protocols

슬라이드는 두 가지 큰 부류를 제시한다.

- Snooping
- Directory

이건 이후 강의의 핵심 축이다.

---

## 4.4 Snooping

슬라이드 bullet:

- Caches keep track of the sharing status of all blocks
- No centralized state is kept
- Cache controllers snoop shared interconnect (typically shared bus) to track when a requested block exists in the cache

뜻:

snooping은 모든 캐시가 **공유 interconnect를 감시(snoop)** 하면서 coherence를 맞추는 방식이다.

즉 어떤 캐시가 bus에 요청을 올리면,  
다른 캐시들도 그걸 다 보고:

- "나 그 block 가지고 있나?"
- "invalidate 해야 하나?"
- "최신 데이터를 내가 제공해야 하나?"

를 판단한다.

### 핵심 특징 1: No centralized state is kept

어디 한 군데에서 모든 block의 sharer 정보를 중앙집중적으로 관리하지 않는다.

즉 중앙 디렉터리 같은 게 없다.

대신 각 캐시가 자기 block 상태를 들고 있고,  
공유 bus/interconnect를 계속 본다.

### 핵심 특징 2: snoop shared interconnect

bus 같은 공유 interconnect가 있어야 모든 캐시가 같은 요청을 관찰할 수 있다.

그래서 snooping은 대개:

- shared bus
- broadcast 가능한 interconnect

와 잘 맞는다.

### 장점

- 구조가 직관적
- 작은 시스템에 적합
- 중앙 상태 관리가 필요 없음

### 단점

- broadcast 기반이라 scalability가 나쁨
- 코어 수가 많아지면 bus/interconnect 부담이 커짐

> [!important]
> **snooping = 모두가 bus를 같이 보고 판단하는 방식**  
> 간단하고 직관적이지만, 큰 시스템으로 가면 broadcast 부담이 커진다.

---

## 4.5 Directory

슬라이드 bullet:

- Sharing status of any block in memory is kept in one location

뜻:

directory 방식에서는 어떤 메모리 블록을 누가 가지고 있는지 정보를  
한 곳(**directory entry**)에 저장해 둔다.

즉 중앙 혹은 분산된 디렉터리 구조가 있어서:

- sharer가 누구인지
- owner가 누구인지
- dirty인지 clean인지

를 추적한다.

### 왜 필요하냐

큰 시스템에서는 snooping처럼 모든 요청을 broadcast하는 것이 너무 비싸다.

그래서 directory는 필요한 대상에게만 메시지를 보낸다.

예:

이 블록을 가진 캐시가 `P1`, `P3`뿐이면

- invalidate도 `P1`, `P3`에게만 보냄

즉 더 scalable하다.

### 장점

- broadcast 줄일 수 있음
- 대규모 시스템에 적합
- NUMA나 distributed memory와 잘 맞음

### 단점

- directory 저장 비용 필요
- 프로토콜이 더 복잡함
- 추가 메시지와 상태 관리 필요

> [!important]
> **directory = 누가 그 block을 가지고 있는지 한 곳에서 추적하는 방식**  
> 복잡하지만 large-scale system에 더 적합하다.

---

## 5. Cache Coherence Example

이 슬라이드는 coherence가 없으면 무슨 일이 생기는지 표로 보여주는 예제다.

---

## 5.1 표 해석

### 초기 상태 (time 0)

- Memory location `X = 1`
- `A` cache 없음
- `B` cache 없음

### Time 1: Processor A reads X

`A`가 `X`를 읽는다.

결과:

- `A` cache: `1`
- `B` cache: 없음
- Memory: `1`

### Time 2: Processor B reads X

`B`도 `X`를 읽는다.

결과:

- `A` cache: `1`
- `B` cache: `1`
- Memory: `1`

즉 같은 데이터 `X = 1`이 `A` cache와 `B` cache 양쪽에 복사되었다.

### Time 3: Processor A stores 0 into X

`A`가 `X`를 `0`으로 바꾼다.

결과 표에서는:

- `A` cache: `0`
- `B` cache: `1`
- Memory: `0` 또는 시스템에 따라 write-back 전까지 old일 수 있지만, 슬라이드의 요지는 `B`가 stale하다는 점

핵심은:

- `A`의 복사본은 새 값
- `B`의 복사본은 여전히 옛날 값 `1`

즉 `B` cache가 **stale data**를 가지고 있게 된다.

---

## 5.2 슬라이드의 결론

### Without cache coherence, Processor B’s cache has stale data

이게 coherence 문제의 본질이다.

즉 coherence가 없으면  
각 캐시는 자기 마음대로 값이 달라질 수 있고,  
그러면 shared memory라는 프로그래밍 모델이 깨진다.

---

## 5.3 How to enforce coherence?

슬라이드는 두 가지 방법을 소개한다.

- `B`’s cache line is invalidated (**write-invalidate protocols**)
- `B`’s cache line is updated by `A`’s write (**write-update protocols**)

이게 바로 다음 슬라이드 내용과 연결된다.

> [!summary]
> stale copy를 처리하는 큰 전략은 두 가지다:
>
> - **Invalidate**: 다른 copy를 못 쓰게 만든다
> - **Update**: 다른 copy도 최신 값으로 바꾼다

---

## 6. Invalidate vs. Update Protocols

이 슬라이드는 coherence를 유지하는 두 가지 큰 전략을 비교한다.

---

## 6.1 Write-invalidate protocols

이 방식에서는 어떤 프로세서가 write하려고 하면,  
다른 캐시가 가지고 있는 복사본들을 **무효화(invalidate)** 한다.

### Guarantees only one writer has a valid copy of a block

이 문장이 핵심이다.

뜻:

어떤 블록에 대해 write하려는 순간,  
유효한 복사본을 가진 writer는 하나뿐이어야 한다.

즉 write를 하려는 프로세서는 그 블록의 **exclusive ownership**을 가져야 하고,  
다른 캐시들은 그 복사본을 버려야 한다.

### Read requests issue a "Get Shared" (GetS) request

읽고 싶을 때는 보통 `Get Shared (GetS)`를 보낸다.

뜻:

- "이 블록 읽고 싶다"
- "공유 가능한 copy 하나 달라"

그래서 읽기는 여러 캐시가 **shared 상태**로 같이 가질 수 있다.

### When a processor wants to write to a cache block, it issues a "Get Exclusive" (GetX) request

쓰고 싶을 때는 보통 `Get Exclusive (GetX)`를 보낸다.

뜻:

- "이 블록에 쓰고 싶다"
- "나만 유효한 copy를 갖게 해라"

그래서 다른 캐시들은 자기 copy를 invalidate해야 한다.

### Subsequent writes from the same processor are done locally in the cache

이건 write-invalidate의 큰 장점이다.

한 번 `GetX`로 ownership을 얻고 나면,  
그 이후 같은 프로세서의 연속 write는 로컬 캐시 안에서 처리하면 된다.

즉:

- 첫 write 때만 비싸고
- 그 다음 write들은 싸다

> [!important]
> **write-invalidate의 핵심 철학:**  
> "다른 애들 copy는 지워. 내가 혼자 쓸게."

---

## 6.2 Write-update protocols

이 방식에서는 어떤 프로세서가 write할 때,  
다른 캐시들이 가진 valid copy도 같이 **업데이트**해 준다.

슬라이드 문장:

> When a processor writes to a block, it sends data to all other processors with valid copies

즉 invalidate하지 않고,  
다른 캐시에도 새 값을 push한다.

> [!important]
> **write-update의 핵심 철학:**  
> "내가 값을 바꿨으니, 너희 copy도 전부 새 값으로 고쳐."

---

## 6.3 두 방식의 직관적 차이

### Write-invalidate

**"너희 copy는 버려. 나 혼자 쓸게."**

### Write-update

**"내가 값 바꿨으니까 너희 copy도 새 값으로 고쳐."**

---

## 7. Invalidate vs Update의 장단점

슬라이드에는 `Pros and cons?`만 적혀 있고 자세한 설명은 안 적혀 있으니까, 여기서 상세히 정리한다.

---

## 7.1 Write-invalidate의 장점

### 장점 1: repeated writes에 강함

한 프로세서가 같은 block을 여러 번 연속으로 쓰면,  
처음 한 번 ownership만 얻으면 그 뒤는 로컬에서 계속 write 가능하다.

즉 write-heavy 패턴에 유리하다.

### 장점 2: 불필요한 데이터 전송이 적음

다른 프로세서가 당장 그 값을 읽지 않을 수도 있는데,  
write-update는 어쨌든 다 보내야 한다.

invalidate는 그냥 "버려"만 보내므로 traffic이 적을 수 있다.

### 장점 3: 현대 시스템에서 더 일반적

실제 상용 프로토콜 대부분은 write-invalidate 계열이다.

---

## 7.2 Write-invalidate의 단점

### 단점 1: 이후 다른 프로세서가 read하면 miss 발생

다른 캐시들은 invalidate되었으므로,  
나중에 다시 읽으려면 miss가 난다.

### 단점 2: ping-pong 가능

둘 이상의 프로세서가 번갈아 같은 block에 write하면  
ownership이 계속 왔다 갔다 하면서 traffic이 커질 수 있다.

---

## 7.3 Write-update의 장점

### 장점 1: readers가 많은 경우 miss를 줄일 수 있음

write 후에 다른 프로세서들이 곧바로 그 값을 읽는다면,  
이미 업데이트된 copy를 갖고 있으므로 read miss를 줄일 수 있다.

### 장점 2: stale copy 문제를 직접적으로 해결

모든 valid copy를 즉시 최신 값으로 바꾸므로,  
읽는 쪽은 계속 유효한 copy를 들고 있다.

---

## 7.4 Write-update의 단점

### 단점 1: traffic가 매우 커질 수 있음

write할 때마다 모든 sharer에게 새 데이터를 보내야 한다.

shared copy가 많으면 bandwidth 낭비가 심하다.

### 단점 2: reader가 실제로 그 값을 안 쓸 수도 있음

다른 프로세서가 그 block을 다시 곧바로 읽지 않을 수도 있는데,  
업데이트는 미리 다 보내므로 낭비가 생길 수 있다.

### 단점 3: repeated writes에 비효율적

한 writer가 같은 값을 여러 번 바꿀 때마다 계속 남들에게 push해야 하므로 비효율적이다.

> [!summary]
> 보통은:
>
> - **invalidate** = write-heavy에 유리
> - **update** = readers가 매우 많고 곧바로 읽는 경우 유리할 수 있음
>
> 그래서 실제 시스템은 대체로 **write-invalidate**를 더 많이 쓴다.

---

## 8. 슬라이드 묶음 전체의 큰 흐름

이 5장 슬라이드를 하나의 논리로 묶으면 이렇게 된다.

### 8.1 왜 coherence가 필요하냐

여러 캐시가 같은 memory location의 복사본을 동시에 가질 수 있기 때문이다.

### 8.2 coherence는 무엇을 보장해야 하냐

적어도 같은 location에 대해:

- 같은 프로세서는 자기 write 뒤에 자기 값을 읽어야 하고
- 다른 프로세서도 충분한 시간 뒤엔 최신 값을 봐야 하며
- 같은 location에 대한 write 순서는 모두에게 같아야 한다

### 8.3 coherence와 consistency는 어떻게 다르냐

- coherence는 같은 주소
- consistency는 다른 주소들 사이의 순서

### 8.4 coherence protocol은 어떻게 나뉘냐

- **Snooping**: 모두가 shared interconnect를 감시
- **Directory**: 블록 상태 정보를 한 위치에 저장

### 8.5 stale copy는 어떻게 해결하냐

두 가지 큰 전략이 있다.

- **Invalidate**: 다른 copy를 무효화
- **Update**: 다른 copy를 새 값으로 갱신

그리고 실제로는 **write-invalidate가 훨씬 더 흔하다.**

---

## 9. 매우 중요한 시험용 정리

### 9.1 Cache coherence의 정의

cache coherence는 **같은 memory location**에 대한 read/write 동작이 프로세서들 사이에서 일관되게 보이도록 보장하는 하드웨어 메커니즘이다.

### 9.2 세 가지 coherence 조건

- **Program Order**: 같은 프로세서의 write 후 read는 자기 값을 봐야 함
- **Coherent View of Memory**: 다른 프로세서의 write도 시간이 지나면 보여야 함
- **Write Serialization**: 같은 location에 대한 write 순서는 모두에게 같아야 함

### 9.3 Coherence vs Consistency

- **Coherence**: one location
- **Consistency**: ordering across multiple locations

### 9.4 Snooping vs Directory

- **Snooping**: 중앙 상태 없음, shared interconnect 감시
- **Directory**: sharing 정보를 한 곳에서 관리

### 9.5 Invalidate vs Update

- **Invalidate**: 다른 copy를 버리게 함, repeated writes에 유리
- **Update**: 다른 copy를 최신 값으로 갱신, sharer가 많으면 traffic 증가

> [!tip]
> 시험에서는 이 다섯 묶음을 연결해서 말할 수 있어야 한다:
>
> **왜 coherence 필요? → coherence가 보장할 조건 → coherence vs consistency → snooping vs directory → invalidate vs update**

---

## 10. 예시 코드로 다시 보기

### 예시 1: stale copy 문제

초기 상태:

    # Initially: X = 1

`P1`:

    r1 = X   # gets 1 into P1 cache

`P2`:

    r2 = X   # gets 1 into P2 cache

그 다음 `P1`:

    X = 0    # P1 cache now has 0

이 상태에서 coherence가 없으면 `P2`는 여전히 `1`을 갖고 있을 수 있다.

---

### 예시 2: invalidate 방식

    P1 wants to write X
    -> send GetX
    -> invalidate P2's copy
    -> P1 becomes exclusive owner
    -> write locally

---

### 예시 3: update 방식

    P1 writes X = 0
    -> send new value to all caches with valid copies
    -> P2's cached copy of X becomes 0 too

---

## 11. 직관으로 이해하기

### invalidate

칠판에 여러 사람이 같은 메모를 복사해 들고 있는데,  
한 사람이 내용을 고치려고 한다.

그러면:

**"다른 사람들 메모는 다 찢어. 내가 최신본 들고 있을게."**

이게 invalidate다.

### update

반대로:

**"내가 내용을 바꿨으니, 너희 메모도 전부 이 새 내용으로 고쳐."**

이게 update다.

> [!example]
> 그래서 직관적으로:
>
> - **invalidate = 독점권을 얻는 방식**
> - **update = 복사본들을 계속 동기화하는 방식**
>
> 이라고 생각하면 된다.

---

## 12. 다음 내용과의 연결

이 슬라이드 묶음 다음에는 보통:

- write-invalidate 프로토콜이 실제로 어떻게 동작하는지
- MSI / MESI / MOESI 같은 상태 전이
- snooping / directory 구현
- true sharing / false sharing
- coherence miss

같은 내용으로 이어진다.

즉 지금 이 슬라이드들은 그 뒤의 **구체적인 프로토콜을 배우기 위한 개념적 기초**다.

> [!summary]
> 한 줄로 끝내면:
>
> **Cache coherence는 여러 캐시에 존재하는 같은 데이터의 복사본들이 서로 모순되지 않도록 보장하는 하드웨어 메커니즘이며, 이를 위해 snooping/directory 및 invalidate/update 같은 전략이 사용된다.**
>