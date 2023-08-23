# 쿼리 조회결과 NULL일때 Service method 처리 방식 예제

개발내용 : session에서 user정보 가져와서 관리자/조회 권한이 있는지 확인하기

- Controller.java

  - return type 이 Boolean (True/False)

  ```java
  @GetMapping(value = "/auth/isMngrOrInq")
  public Boolean isMngrOrInq(HttpServletRequest req) {
    // session정보 가져오기
    UserDataDTO sessionData = SessionManager.getCurrentUserData(req);
    String userId = sessionData.getUserId();

    return authService.hasAuth(userId, "MNGR") || authService.hasAuth(userId, "INQ");
  }
  ```

- Service.java

  - 쿼리 조회 결과가 있으면 True를 return

  ```java
  private boolean hasAuth(String userId, String authId) {
    HashMap<String,String> inputParam = new HashMap<>();
    inputParam.put("userId", userId);
    inputParam.put("authId", authId);
    // 쿼리 실행
    String result = commonDao.selectOne("UserAuth.selectUserAuth", inputParam);
    return result != null;
  }
  ```

  ​