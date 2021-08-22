### 크기가 고정된 버퍼와 그 버퍼에 접근하기 위한 인덱스를 공유하는 생산자와 소비자 스레드를 작성하세요. 생산자는 버퍼에 숫자를 집어넣고 소비자는 숫자를 제거해야 합니다. 숫자가 추가되거나 제거되는 순서는 중요하지 않습니다.

1. The consumer has just read the variable itemCount, noticed it's zero and is just about to move inside the if block.

2. Just before calling sleep, the consumer is interrupted and the producer is resumed.

3. The producer creates an item, puts it into the buffer, and increases itemCount.

4. Because the buffer was empty prior to the last addition, the producer tries to wake up the consumer.

5. Unfortunately, the consumer wasn't yet sleeping, and the wakeup call is lost. When the consumer resumes, it goes to sleep and will never be awakened again.

6. This is because the consumer is only awakened by the producer when itemCount is equal to 1.

7. The producer will loop until the buffer is full, after which it will also go to sleep.

```c
int itemCount = 0;

procedure producer()
{
    while (true)
    {
        item = produceItem();

        if (itemCount == BUFFER_SIZE)
        {
            sleep();
        }

        putItemIntoBuffer(item);
        itemCount = itemCount + 1;

        if (itemCount == 1)
        {
            wakeup(consumer);
        }
    }
}

procedure consumer()
{
    while (true)
    {

        if (itemCount == 0)
        {
            sleep();
        }

        item = removeItemFromBuffer();
        itemCount = itemCount - 1;

        if (itemCount == BUFFER_SIZE - 1)
        {
            wakeup(producer);
        }

        consumeItem(item);
    }
}
```

| 참고

- [producer-consumer-problem](https://en.wikipedia.org/wiki/Producer%E2%80%93consumer_problem)
