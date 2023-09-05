## Mybatis 연동방법

1. `pom.xml` 에서 mybatis dependency 추가

   ```xml
   <!--  MyBatis 관련 라이브러리 추가  -->
           <dependency>
               <groupId>org.mybatis.spring.boot</groupId>
               <artifactId>mybatis-spring-boot-starter</artifactId>
               <version>2.1.4</version>
           </dependency>
   ```

   ​

2. mybatis 관련 설정정보 작성

   - 방법1) mybatis Config class를 생성해서 BatchApplication클래스처럼 관련 bean을 설정
   - 방법2) application.yml이나 application-local.properties에서 설정
   - (처음에 방법1,2를 동시에 적용하려했더니 충돌오류가 발생하여, 방법2를 적용함.)



### application.yml이나 application-local.properties에서 설정하는 방법

`application.yml`

```yaml
mybatis:
#  config-location: classpath:mybatis-config.xml   # 방법1 
  mapper-locations: classpath:/sql/mysql/**/sql-*.xml   
  executorType: BATCH
  configuration:
    multiple-result-sets-enabled: false
    map-underscore-to-camel-case: true
    call-setters-on-nulls: true    
    jdbc-type-for-null: varchar
    default-fetch-size: 500  
```



### Mybatis Reader/Writer 예시코드

```java
@Configuration
public class TestJobConfig extends BaseJob {
 
  @Autowired
  private SqlSessionFactory sqlSessionFactory;

  // Reader 예제
  @Bean
  @StepScope
  public MyBatisCursorItemReader<EntrDTO> EntrReader() throws Exception {

    MyBatisCursorItemReader<EntrDTO> reader = new MyBatisCursorItemReader<EntrDTO>();
    reader.setSqlSessionFactory(sqlSessionFactory);
    reader.setQueryId("EntrMapper.selectEntrList");
    return reader;
  }

  // Writer 예제
  public MyBatisBatchItemWriter<EntrDTO> chgEntrMWriter() throws Exception {
  
    MyBatisBatchItemWriter<EntrDTO> writer = new MyBatisBatchItemWriterBuilder<EntrDTO>()
                                                      .sqlSessionFactory(sqlSessionFactory)
                                                      .statementId("EntrMapper.updateEntr")
                                                      .build();
    return writer;
  }
}
```

