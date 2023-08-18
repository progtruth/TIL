## ModelMapper 사용하여 DTO to DTO 깔끔하게 매핑하는 방법

공통 속성을 여러개 가지고 있는 DTO 2개가 있다면 (예. Hist -> Master), `ModelMapper`를 사용하여 일일히 setter를 사용하지 않고 한 DTO에서 다른 DTO로 매핑할 수 있다.

### ModelMapper 사용방법

1. pom.xml 에 dependency 추가

   ```xml
   <!--  DTO 매핑을 위한 modelmapper 라이브러리 추가  -->
   <dependency>
       <groupId>org.modelmapper</groupId>
       <artifactId>modelmapper</artifactId>
        <version>2.4.4</version>
   </dependency>
   ```


2. `ModelMapperConfig` configuration 추가

   ```java
   @Configuration
   public class ModelMapperConfig {

       @Bean
       public ModelMapper modelMapper() {
           return new ModelMapper();
       }
   }
   ```

3. 사용 

   - batch의 processor에서 활용한 예제코드

   ```java
   @Slf4j
   @Data
   public class ExampleProcessor extends BaseProcessor<OldDTO, NewDTO> {

       private final ModelMapper modelMapper; // modelMapper를 initialize해주는 생성자가 필요, @data annotation을 사용해도됨

       @Override
       public NewDTO run(OldDTO oldDTO) throws Exception {

           // OldDTO -> NewDTO로 매핑
           NewDTO newDTO = modelMapper.map(oldDTO , NewDTO.class);
           return newDTO;
       }
   }
   ```

   ​