## 데드락이란?

프로세스가 자원을 얻지 못해 다음 처리를 하지 못하는 상태로, ‘교착 상태’라고도 하며 시스템적으로 한정된 자원을 여러 곳에서 사용하려고 할 때 발생합니다.

![](https://t1.daumcdn.net/cfile/tistory/243E89355714C26E28)

발생조건(아래 4가지를 채우면 교착상태가 발생하지만 반대로 하나라도 안채우면 교착상태가 발생하지 않는다는것은 필요충분 조건이 아닙니다.)

1. 상호배제(Mutual Exclusion)

- 자원은 한 번에 한 프로세스만이 사용할 수 있어야 한다.

2. 점유대기(Hold and Wait)

- A process must be holding a resource and waiting for another/ 프로세스가 자원을 요청할 때 다른 자원을 점유하고 있지 않음을 보장해야 한다.

3. 비선점

- 다른 프로세스에 할당된 자원은 사용이 끝날 때까지 강제로 빼앗을 수 없어야 한다.

4. 순환대기

- 프로세스의 집합 {P0, P1, ,…Pn}에서 P0는 P1이 점유한 자원을 대기하고 P1은 P2가 점유한 자원을 대기하고 P2…Pn-1은 Pn이 점유한 자원을 대기하며 Pn은 P0가 점유한 자원을 요구해야 한다.

| 참고

- [데드락](https://jwprogramming.tistory.com/12)
- [데드락2](https://slidesplayer.org/slide/17155750/)
