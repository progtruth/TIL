## Git Branch 정책

현재 운영중인 프로젝트(1차 오픈완료)에서 sub프로젝트(2차 오픈대상)가 진행되고 있고, 그외에 현재 운영환경에 비정기적으로 반영되어야하는 개발건이 있을경우 하기 정책을 사용함.

### Git Branch 목록

- `develop`: 테스트1환경에 배포되는 브랜치 (1 + 2차 수정사항 전부 포함)
- `master`: 테스트2환경에 배포되는 브랜치 (1차 수정사항만 포함)
- `prod`: 운영환경에 배포되는 브랜치 (master 브랜치로 자동 배포)
- `hotfix`: 1차 오픈에 대한 수정사항만 배포하는 브랜치

```
           hotfix
             |
feature -> develop -> master -> prod
```

### 2차오픈소스 분리를 위한 Git Branch 정책

- 2차오픈대상 신규 기능들은 `feature` branch를 새로 따서,  `develop` branch로 Merge Request를 올린다.
- 1차오픈 수정건 : local/hotfix -> origin/hotfix -> origin/master -> 테스트2환경 배포 -> 운영환경 배포
- 2차오픈 개발건 : local/feature_xx를 develop으로부터 생성 -> origin/develop로 merge request -> origin/develop merge -> 테스트1환경 배포
- 1차오픈 수정건을 2차오픈에 반영 : origin/master -> origin/develop -> 테스트1환경 배포 (차후 테스트2환경 테스트를 위해 origin/develop -> origin/master 로 배포 예정)

#### 1차오픈후, local/develop 브랜치 삭제 후 새로 origin/develop 브랜치 받아서 개발

1. 기존 local 의 develop 삭제
2. `git fetch origin --prune`실행해서 remote branch들 fetch
3. 2차오픈대상 개발건(기능별) feature_xx branch 생성 후, 개발
4. origin/develop에 merge request 후 merge 
5. 테스트1환경으로 배포 요청 


#### (참고) prune 옵션

- Git Remote에 없는 Branch에 대한 Local Remote Tracking을 없앨 때 사용
- `git fetch` 명령어를 수행하게 되면 remote에 있는 branch를 가져오는데, 여기에 `--prune`옵션을 주게되면, remote에 존재하지 않는 브랜치들에 대한 reference를 local에서 삭제하는 작업도 진행된다.


#### (참고) branch의 특정 commit상태로 new branch 생성 명령어

```
// 브랜치A의 특정 커밋 상태(commit_id)로 새로운 브랜치B 만들기
$ git checkout -b <new_branch> <commit_id>
```

