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
title = "SOQL 기본문법(08)"

#설명
description = "Date Functions (날짜 함수)"

#공개: false / 비공개: true
draft = false


image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
comments = true
date = 2019-05-28T13:31:56+09:00
+++

<!-- 게시글 내용 -->

### Date Functions (날짜 함수)
SOQL 쿼리의 날짜 함수를 사용하면 일별, 월별 또는 회계 연도와 같은 날짜 기간별로 데이터를 그룹화하거나 필터링 할 수 있습니다.

```sql
  SELECT CALENDAR_YEAR(CreatedDate), SUM(Amount)
    FROM Opportunity
GROUP BY CALENDAR_YEAR(CreatedDate)
```
Date Function | Description | Examples
---|:---|:---
CALENDAR_MONTH() | 월 (숫자) | 1 for January <br/> 12 for December
CALENDAR_QUARTER() | 분기 (숫자) | 1 for January 1 through March 31<br/>2 for April 1 through June 30<br/>3 for July 1 through September 30<br/>4 for October 1 through December 31
CALENDAR_YEAR() | 년 (숫자) | 2009
DAY_IN_MONTH() | 일자 (숫자) | 20 for February 20 
DAY_IN_WEEK() | 요일 (숫자) | 1 for Sunday <br/> 7 for Saturday 
DAY_IN_YEAR() | 년의 일자 (숫자) | 32 for February 1
DAY_ONLY() | DateTime 형식 <br/> 날짜만 반환 | 2009-09-22 for September 22, 2009<br/>You can only use DAY_ONLY() with dateTime fields.
FISCAL_MONTH() | 회계 월 (숫자) | If your fiscal year starts in March: <br/> 1 for March <br/> 12 for February
FISCAL_QUARTER() | 회계 분기 (숫자) | If your fiscal year starts in July:<br/> 1 for July 15<br/> 4 for June 6
FISCAL_YEAR() | 회계 년 (숫자) | 2009
HOUR_IN_DAY() | 시간 (숫자) | 18 for a time of 18:23:10 <br/> You can only use HOUR_IN_DAY() with dateTime fields.
WEEK_IN_MONTH() | 월의 주차 (숫자) | 2 for April 10 <br/>The first week is from the first through the seventh day of the month.
WEEK_IN_YEAR() | 년의 주차 (숫자) | 1 for January 3 <br/>The first week is from January 1 through January 7.

```sql
( OK )
SELECT CreatedDate, Amount
  FROM Opportunity
 WHERE CALENDAR_YEAR(CreatedDate) = 2009

( Error )
SELECT CreatedDate, Amount
  FROM Opportunity
 WHERE CALENDAR_YEAR(CreatedDate) = THIS_YEAR

( Error )
SELECT CALENDAR_YEAR(CreatedDate), Amount
  FROM Opportunity

( OK )
  SELECT CALENDAR_YEAR(CloseDate)
    FROM Opportunity
GROUP BY CALENDAR_YEAR(CloseDate)
```
###### &nbsp;
### Converting Time Zones in Date Functions (날짜 함수에서 표준 시간대 변환)
```sql
  SELECT HOUR_IN_DAY(convertTimezone(CreatedDate)), SUM(Amount)
    FROM Opportunity
GROUP BY HOUR_IN_DAY(convertTimezone(CreatedDate))
```

###### &nbsp;
### Querying Currency Fields in Multi-Currency Orgs (복수 통화 조직의 통화 필드 쿼리)
통화 필드를 사용자의 통화로 변환하려면 SOQL 쿼리의 SELECT 문에서 convertCurrency ()를 사용하십시오.
```sql
SELECT Id, convertCurrency(AnnualRevenue)
  FROM Account
```
###### &nbsp;
### Understanding Relationship Names (관계 이름 이해하기)
![soql](https://developer.salesforce.com/docs/resources/img/en-us/218.0?doc_id=dev_guides%2Fsoql_sosl%2Fimages%2Frel_basic.gif&folder=soql_sosl)

```sql
SELECT Contact.FirstName, Contact.Account.Name 
  FROM Contact

SELECT Account.Name, 
    (
    SELECT Contact.FirstName, Contact.LastName 
      FROM Account.Contacts
    ) 
  FROM Account
```

###### &nbsp;
### Using Relationship Queries (관계 검색어 사용)
```sql
SELECT Id, Name, Account.Name
  FROM Contact
 WHERE Account.Industry = 'media'

SELECT Name,
    (
    SELECT LastName
      FROM Contacts
    )
  FROM Account

SELECT Name,
    (
    SELECT CreatedBy.Name
    FROM Notes
    )
  FROM Account

SELECT Amount, Id, Name,
    (
    SELECT Quantity, ListPrice, PricebookEntry.UnitPrice, PricebookEntry.Name
      FROM OpportunityLineItems
    )
  FROM Opportunity

SELECT Amount, Id, Name, 
    (
    SELECT Quantity, ListPrice, PriceBookEntry.UnitPrice, PricebookEntry.Name,
           PricebookEntry.product2.Family 
      FROM OpportunityLineItems
    )
  FROM Opportunity
 
SELECT Name,
    (
    SELECT LastName
      FROM Contacts
     WHERE CreatedBy.Alias = 'x'
    )
  FROM Account
 WHERE Industry = 'media'
```

###### &nbsp;
### Understanding Relationship Names, Custom Objects, and Custom Fields
### (관계 이름, 사용자 지정 개체 및 사용자 지정 필드 이해)
Lookup field or Detail의 Parent Field 만들 때 Child Relationship Name 지정
```sql
예) Daughters

SELECT Id, FirstName__c, Mother_of_Child__r.FirstName__c
  FROM Daughter__c
 WHERE Mother_of_Child__r.LastName__c LIKE 'C%'

SELECT LastName__c,
    (
    SELECT LastName__c
      FROM Daughters__r
    )
  FROM Mother__c
```

###### &nbsp;
### Understanding Query Results (쿼리 결과 이해하기)
```sql
SELECT Id, FirstName, LastName, AccountId, Account.Name
  FROM Contact
 WHERE Account.Name LIKE 'Acme%'

SELECT Id, Name,
    (
    SELECT Id, FirstName, LastName
      FROM Contacts
    )
  FROM Account
 WHERE Name LIKE 'Acme%'
```