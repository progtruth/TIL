### 테스트환경에 logback 적용 방법

logback-spring.xml 파일에서 log설정을 관리함.

- logback-spring.xml 
  `springProfile name="dev"` 부분에 (dev가 테스트환경의 profile명)
  `<logger name="testpackage.package" level="debug"/>` 추가
- `testpackage.package` 하위 패키지에서 발생한 debug 레벨 이상의 로그를 모두 log파일에 남긴다.