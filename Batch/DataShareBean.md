## DataShareBean 

- Spring batch에서 Step간 데이터 공유를 위해 사용한다.
- Step의 종류
  - Chunk : ItemReader, ItemProcessor, ItemWriter 로 단계를 나누어 Step을 구성하고, 이 Step들로 하나의 Job을 구성
  - Tasklet : 별도로 단계를 분리하지않고 Job을 구성하는 단순 방식
- 나의 경우에는 Step이 독립적으로 분리되어있으나, 기준날짜나 target 등의 데이터를 step간에 공유가 필요할 듯 하여, `DataShareBean`을 공통클래스로 생성하여 사용하게 되었다.

### DataShareBean Class

- 나의 경우, key를 `String` 으로 설정했다.

```java
@Component
public class DataShareBean<T> {
  private Map<String, T> shareDataMap = new ConcurrentHashMap<String, T>();
  
  public void putData(String key, T data) {
    if (this.shareDataMap != null) {
      this.shareDataMap.put(key, data);
    }
  }
  
  public T getData(String Key) {
    return this.shareDataMap == null ? null : this.shareDataMap.get(key);
  }
  
  public int getSize() {
    return this.shareDataMap == null ? 0 : this.shareDataMap.size();
  }
}
```

### 사용 방법

1. DataShareBean 선언

   - JobConfiguration.java에서 

   ```java
   @Configuration 
   public class TestJobConfig extends BaseJob {
     
     @Autowired
     private DataShareBean<String> dataShareBean;

     public TestJobConfig (DataShareBean<String> dataShareBean) {
       this.dataShareBean = dataShareBean;
     }
   }
   ```

2. putData()나 getData()로 사용

   ```java
   @Configuration 
   public class TestJobConfig extends BaseJob {

     @Bean(name="testJob_Step01")
     public Step testStep01() throws Exception {

       dataShareBean.putData("batchTrgt", batchTrgt);
       :
     }

     @Bean(name="testJob_Step02")
     public Step testStep02() throws Exception {

       log.info("## DataShareBean.batchTrgt = ", dataShareBean.getData("batchTrgt"));
       :
     }
   }
   ```

   ​