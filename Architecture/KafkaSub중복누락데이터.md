## Kafka Sub 할때 중복되거나 누락되는 데이터가 없는가?

- Spring에서 kafka library 활용하여 sub 구현

```java
Consumer<String, String> consumer = new KafkaConsumer<>(properties);

// 연결할 토픽 구독
consumer.subscribe(Collections.singleton(topicName));

ConsumerRecords<String, String> records = consumer.poll(Duration.ofSeconds(5));
```

### `KafkaConsumer.poll(10)` method

- 새로운 레코드가 도착할때까지 무한정 block되어 대기한다.
- parameter로 10을 주면, 지정된 시간동안만 block되고 10초동안 레코드가 도착하지 않은경우, 반환된다.
    - 즉 10은 timeout값이라고 볼수 있다.
- timeout값
    - 양수 : 지정된 시간동안 block 되고, 레코드가 도착하지 않으면 empty 레코드를 반환

### `ConsumerRecords` 객체

- `KafkaConsumer.poll()` 메서드의 return객체 타입
- `poll()` 을 호출하면 kafka서버로부터 메시지를 가져와 `ConsumerRecords` 객체를 반환하는데

    이때 Topic별로 레코드를 포함하는 `Map`객체
- 일반적으로 `ConsumerRecords`에서 레코드를 가져와서 처리하고,
     `KafkaConsumer`가 가져온 <u>마지막 레코드를 기록한다.</u>

    - 마지막 레코드의 위치를 `offset` 이라고 하며,
    - **offset으로 commit한다**고 표현
- 이를 통해 `KafkaConsumer`가 가져온 레코드를 중복으로 가져오지 않도록 한다.

#### offset

- 메시지 각각은 offset번호로 식별한다.
- 각 파티션마다 별도의 offset을 가지며, 컨슈머 그룹의 각 컨슈머마다 <u>파티션의 마지막 offset정보</u>를 기억하고 있음.
- 컨슈머는 offset정보를 통해서 어디까지 메시지를 가져왔는지 확인가능
    - 어디까지 메시지를 읽었는지, 누락없이 모든 메시지를 읽었는지 정확하게 판별가능함




#### (참고 라이브러리)

```java
import java.time.Duration;
import java.util.Collections;
import java.util.Properties;

import org.apache.kafka.clients.consumer.Consumer;
import org.apache.kafka.clients.consumer.ConsumerConfig;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.KafkaConsumer;
```

