## MyBatisBatchItemWriter 에서 update시 변경사항 없으면 오류 발생

오류내용 : update 쿼리를 실행시, update항목이 없으면, 변경사항이 없어서 오류 발생

```
Batch execution returned invalid results. Expected 1 but number of BatchResult objects returned was 3
```

해결방법

- `assertUpdates(false) ` 옵션추가

  ```java
  public MyBatisBatchItemWriter<EntrMDTO> testMWriter() throws Exception {
    	return new MyBatisBatchItemWriterBuilder<TestDTO>()
      .sqlSessionFactory(sqlSessionFactory)
      .statementId("TestMapper.updateTestDTO")
      .assertUpdates(false)	/* 옵션추가 */
      .build();
  };
  ```

  ​