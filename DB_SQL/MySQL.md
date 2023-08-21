## 스키마 내 모든 컬럼 조회

```mysql
select TABLE_NAME, COLUMN_NAME 
from information_schema.columns 
where table_schema = '스키마명' 
and table_name like "%테이블명규칙%" ;
```

