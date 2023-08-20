## FlatFileItemReader 로 파일데이터 읽는 방법

- 배치에서 flatFIle read할때 한글깨짐 해결방법: encoding 옵션으로 MS949를 준다.

```java
@Bean
@StepScope
public FlatFileItemReader<FileDTO> entrFlatFileItemReader() throws Exception, UnexpectedInputException, ParseException, NonTransientResourceException {
  
  String[] fileNames = new String[]{"name1", "name2", "name3"};
  Resource inputFile = new FileSystemResource("/filedir/filename");
  
  return new FlatFileItemReaderBuilder<FileDTO>()
    .name("CustomFlatFileItemReaderStep")
    .delimited()
    .delimiter("|")			/* 구분자로 구분되어있을 경우 사용, default: 콤마 */
    .names(fileNames)		/* 구분자로 나눈 데이터의 명칭 list */
    .targetType(FileDTO.class)	/* 매핑되는 DTO (fileNames의 명칭과 매핑됨) */
    .encoding("MS949")		/* 인코딩옵션 추가 */
    .resource(inputFile)	/* flatFile명 */
    .build();
}
```

