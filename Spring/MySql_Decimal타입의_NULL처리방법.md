## MySql Decimal 타입의 NULL 처리방법

### 오류내용

- 기존에 user_id 필드는 decimal(15,0) 타입이기때문에 Double로 데이터 전처리를 했는데, 
  이를 `java.math.BigDecimal`로 수정 후 NULL 체크를 하면서 오류 발생

  ```java
  userDTO.getUserId().equals(null)
  ```

- DB에서 조회한 decimal 타입의 필드값이 null인 경우, `getUserId()`로 접근하면 `NullPointerException` 오류가 발생

### 해결방안

1. DTO에서 MySql Decimal 타입의 필드들을 `java.math.BigDecimal`로 설정

   ```java
   private BigDecimal userId;
   ```

2. select 쿼리에서 null일 경우의 default값 처리를 한다.

   ```mysql
   select ifnull(user_id, 0) as user_id 
   from user_table
   ```

3. Service에서 null 체크할 때 하기코드처럼 구현.

   ```java
   private final static BigDecimal zero = new BigDecimal(0);

   if (!userDTO.getUserId().equals(zero)) { // decimal 타입의 null값 체크
     user2DTO.setUserId(userDTO.getUserId());
   } 
   ```

   ​