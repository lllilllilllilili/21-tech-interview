## 일관성을 유지해야 하는 DB에서 트랜잭션이 정상적으로 종료되지 않았을 경우 어떻게 처리할 것인가?

"복구는 undo를 통해서 복구하게 됩니다. 즉, rollback을 한다는 의미입니다. 시스템 장애가 발생
하게 되면 undo 데이터도 모두 날라갑니다. 결국 시스템 장애시 redo 데이터를 이용해서 마지막
check point부터 장애까지의 db buffer cache를 복구하게 됩니다. 이게 완료가 되면 undo를
이용해 commit되지 않은 데이터를 모두 rollback함으로써 복구를 완료하게 됩니다. "

요약 : redo -> undo를 살리고 -> db buffer cache 복구-> undo로 다시 생성
(check point가 있는 경우)

없으면 redo복구를 통해 db buffer cache를 복구하게 됩니다.

아래 이미지에서 이 쿼리를 실행 시켰을 때
해당 쿼리를 처리하는데 구매자의 데이터가 필요해지면 데이터 캐시에서 우선 요청하게 됩니다.

```sql
BEGIN TRAN
UPDATE acounts
SET balance = balance - 10000
WHERE user = '구매자';

UPDATE accounts
SET balance = balance + 10000;
WHERE user = '판매자';
COMMIT TRAN
```

![](https://media.vlpt.us/images/syleemk/post/f0cc4b2d-4485-42b8-95a8-e4e4616ca02a/image.png)

없으면 데이터 파일에서 가져오고

![](https://media.vlpt.us/images/syleemk/post/5e330be5-59dc-43d0-b9a0-53b1c9cf8a4d/image.png)

데이터 캐시에 업데이트 하기전에 `로그 캐시`에 담게 되는데 여기서 `redo로그` , `undo로그`를 기록하게 됩니다.

![](https://media.vlpt.us/images/syleemk/post/5750dcd1-ecac-44ca-933f-db139aa135bc/image.png)

기록을 한뒤에 데이터캐시에 업데이트를 하게 됩니다.

트랜잭션이 완료가 되면 redo로그에서 트랜잭션이 commit됬다는 기록을 하게 됩니다.

| 참고

- [trascation](https://velog.io/@syleemk/%EB%A9%B4%EC%A0%91-%EB%8C%80%EB%B9%84-%EC%8A%A4%ED%94%84%EB%A7%81-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EC%A0%84%ED%8C%8C)
