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
title = "SOQL 기본문법(02)"

#설명
description = ""

#비공개: false / 공개: true
draft = false


image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
comments = true
date = 2019-05-24T14:25:50+09:00
+++

<!-- 게시글 내용 -->
### 복수 선택 Picklist 쿼리
|Operator | Description|
|---|:---|
|=|Equals the specified string.|
|!=|Does not equal the specified string.|
|includes|Includes (contains) the specified string.|
|excludes|Excludes (does not contain) the specified string.|
|;|Specifies AND for two or more strings.|

```sql
SELECT Id, MSP1__c FROM CustObj__c WHERE MSP1__c = 'AAA;BBB'
SELECT Id, MSP1__c from CustObj__c WHERE MSP1__c includes ('AAA;BBB','CCC')
```
###### &nbsp;
### 비교 연산자
Operator | Name | Description
---|:---|:---
= | Equals | 
!= | Not equals | 
< | Less than | 
<= | Less or equal | 
> | Greater than | 
>= | Greater or equal | 
LIKE | Like | SELECT AccountId, FirstName, lastname
 |  | FROM Contact
 |  | WHERE lastname LIKE'appl%'
IN | IN | SELECT Name FROM Account
 |  | WHERE BillingState IN ('California', 'New York')
NOT IN | NOT IN | SELECT Name FROM Account	
 |  | WHERE BillingState NOTIN ('California', 'New York')					
INCLUDES| | Applies only to multi-select picklists.			
EXCLUDES| |

###### &nbsp;
### 논리 연산자
Operator | Syntax | Description
---|:---|:---
AND| X AND Y| true if both X and Y are true.
OR| X OR Y| true if either X or Y is true.
NOT| NOT X| true if X is false.


