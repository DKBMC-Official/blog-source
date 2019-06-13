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
title = "SOQL 기본문법(07)"

#설명
description = "Aggregate Functions"

#프로필 사진 (사진: 자신의 한글 이름 입력 / 아바타이미지: 1~51 숫자 입력)
avatar = "차용진"

#공개: false / 비공개: true
draft = false


image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
comments = true
date = 2019-05-28T13:09:45+09:00
+++

<!-- 게시글 내용 -->
### Aggregate Functions
분석을 위해 보고서를 생성하려면 SOQL 쿼리의 GROUP BY 절에 집계 함수를 사용합니다.<br/>
집계 함수에는 AVG (), COUNT (), MIN (), MAX (), SUM () 등이 있습니다.

Aggregate Function | Description
---|:---
AVG() | SELECT CampaignId, AVG(Amount) <br/> FROM Opportunity <br/> GROUP BY CampaignId
COUNT() and COUNT(fieldName) | SELECT COUNT() <br/> FROM Account <br/> WHERE Name LIKE 'a%’ <br/> <br/> SELECT COUNT(Id) <br/> FROM Account <br/> WHERE Name LIKE 'a%’ <br/> GROUP BY 절을 사용하는 경우 COUNT () 대신 COUNT (fieldName)를 사용하십시오.
COUNT_DISTINCT() | SELECT COUNT_DISTINCT(Company) <br/> FROM Lead
MIN() | SELECT MIN(CreatedDate), FirstName, LastName <br/> FROM Contact <br/>GROUP BY FirstName, LastName
MAX() | SELECT Name, MAX(BudgetedCost) <br/> FROM Campaign <br/> GROUP BY Name
SUM() | SELECT SUM(Amount) <br/> FROM Opportunity <br/> WHERE IsClosed = falseAND Probability > 60

_집계 함수를 사용하지만 GROUP BY 절을 사용하지 않는 쿼리에서는 LIMIT 절을 사용할 수 없습니다._
```sql
( Error )
SELECT MAX(CreatedDate)
  FROM Account 
 LIMIT 1
```

> 집계 함수에서 필드 유형 지원

Data Type | AVG() | COUNT() | COUNT_DISTINCT() | MIN() | MAX() | SUM()
---|:---|:---|:---|:---|:---|:---
base64 | No | No | No | No | No | No
boolean | No | No | No | No | No | No
byte | No | No | No | No | No | No
date | No | Yes | Yes | Yes | Yes | No
dateTime | No | Yes | Yes | Yes | Yes | No
double | Yes | Yes | Yes | Yes | Yes | Yes
int | Yes | Yes | Yes | Yes | Yes | Yes
string | No | Yes | Yes | Yes | Yes | No
time | No | No | No | No | No | No
address | No | No | No | No | No | No
anyType | No | No | No | No | No | No
calculated | Depends on data type* | Depends on data type* | Depends on data type* | Depends on data type* | Depends on data type* | Depends on data type*
combobox | No | Yes | Yes | Yes | Yes | No
currency** | Yes | Yes | Yes | Yes | Yes | Yes
DataCategoryGroupReference | No | Yes | Yes | Yes | Yes | No
email | No | Yes | Yes | Yes | Yes | No
encryptedstring | No | No | No | No | No | No
location | No | No | No | No | No | No
ID | No | Yes | Yes | Yes | Yes | No
masterrecord | No | Yes | Yes | Yes | Yes | No
multipicklist | No | No | No | No | No | No
percent | Yes| Yes | Yes | Yes | Yes | Yes
phone | No | Yes | Yes | Yes | Yes | No
picklist | No | Yes | Yes | Yes | Yes | No
reference | No | Yes | Yes | Yes | Yes | No
textarea | No | Yes | Yes | Yes | Yes | No
url | No | Yes | Yes | Yes | Yes | No

