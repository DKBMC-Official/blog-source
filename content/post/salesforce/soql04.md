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
title = "SOQL 기본문법(04)"

#설명
description = "범위 사용"

#프로필 사진 (사진: 자신의 한글 이름 입력 / 아바타이미지: 1~51 숫자 입력)
avatar = "차용진"

#비공개: false / 공개: true
draft = false


image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
comments = true
date = 2019-05-27T11:20:35+09:00
+++

<!-- 게시글 내용 -->

### 범위 사용 
###### &nbsp;
#### USING SCOPE
filterScope | Description
---|:---
Delegated | Filter for records delegated to another user for action. <br/> For example, a query could filter for only delegated Task records.
Everything | Filter for all records.
Mine | Filter for records owned by the user running the query.
MineAndMyGroups | Filter for records assigned to the user running the query and the user’s queues.
My_Territory | Filter for records in the territory of the user running the query. <br/> This option is available if territory management is enabled for your organization.
My_Team_Territory | Filter for records in the territory of the team of the user running the query. <br/> This option is available if territory management is enabled for your organization.
Team | Filter for records assigned to a team, such as an Account team.

```sql
SELECT Id, Name 
  FROM Account USINGSCOPE Mine
```
###### &nbsp;
#### ORDER BY
```sql
[ ORDER BY fieldOrderByList {ASC|DESC} [NULLS {FIRST|LAST}] ]
```

```sql
  SELECT Name
    FROM Account
ORDER BY Name DESCNULLSLAST

  SELECT Id, CaseNumber, Account.Id, Account.Name
    FROM Case
ORDER BY Account.Name

  SELECT Name
    FROM Account
   WHERE industry = 'media'
ORDER BY BillingPostalCode ASCNULLSLASTLIMIT125
```

###### &nbsp;
#### LIMIT
```sql
SELECT Name
  FROM Account
 WHERE Industry = 'Media'LIMIT125
```

###### &nbsp;
#### OFFSET
```sql
  SELECT Name
    FROM Merchandise__c
   WHERE Price__c > 5.0
ORDER BY Name
   LIMIT 100
  OFFSET 10
```
##### OFFSET 사용시 고려 사항

> 1. The maximum offset is 2,000 rows. Requesting an offset greater than 2,000 results in a NUMBER_OUTSIDE_VALID_RANGE error.
> 2. OFFSET is intended to be used in a top-level query, and is not allowed in most subqueries, so the following query is invalid and returns a MALFORMED_QUERY error:


```sql
( Error )
  SELECT Name, Id
    FROM Merchandise__c
   WHERE Id IN
    (
         SELECT Id
           FROM Discontinued_Merchandise__c
          LIMIT 100
        OFF SET 20
    )
ORDER BY Name

( OK )
  SELECT Name, Id
    (
        SELECT Name 
          FROM Opportunity LIMIT10 OFFSET 2
    )
    FROM Account
ORDER BY Name
   LIMIT 1
```
