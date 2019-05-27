+++
#카테고리
categories = [
    "SOQL",
]

#작성자
authors = [
    "Yong-Jin",
]

#제목
title = "SOQL 기본문법(01)"

#설명
description = ""

#공개: false / 비공개: true
draft = false


image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
comments = true
date = 2019-05-24T14:17:04+09:00
+++

<!-- 게시글 내용 -->

### SOQL SELECT 구문 
```sql
SELECT fieldList [subquery][...]
[TYPEOF typeOfField whenExpression[...] elseExpression END][...]
FROM objectType[,...]
    [USINGSCOPE filterScope]
[WHERE conditionExpression]
[WITH [DATA CATEGORY] filteringExpression]
[GROUPBY {fieldGroupByList|ROLLUP (fieldSubtotalGroupByList)|CUBE (fieldSubtotalGroupByList)}
    [HAVING havingConditionExpression] ]
[ORDERBY fieldOrderByList {ASC|DESC} [NULLS {FIRST|LAST}] ]
[LIMIT numberOfRowsToReturn]
[OFFSET numberOfRowsToSkip]
[FOR {VIEW  | REFERENCE}[,...] ]
      [ UPDATE {TRACKING|VIEWSTAT}[,...] ]
```

###### &nbsp;
### SOQL 쿼리에서 null 사용
```sql
SELECT AccountId
  FROM Event
 WHERE ActivityDate != null

SELECT Id
  FROM Case
 WHERE Contact.LastName = null
```

###### &nbsp;
### Boolean(참/거짓) 필드 필터링
```sql
WHERE BooleanField = TRUE
WHERE BooleanField = FALSE 
```