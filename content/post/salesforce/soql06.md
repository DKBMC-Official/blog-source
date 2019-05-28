+++
#카테고리
categories = [
    "SQOL",
]

#작성자
authors = [
    "Yong-Jin",
]

#제목
title = "SQOL 기본문법(06)"

#설명
description = "HAVING"

#공개: false / 비공개: true
draft = false


image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
comments = true
date = 2019-05-28T11:12:45+09:00
+++

<!-- 게시글 내용 -->

### HAVING
__집계 함수가 반환하는 결과를 필터링__

```sql
  SELECT LeadSource, COUNT(Name)
    FROM Lead
GROUP BY LeadSource
  HAVING COUNT(Name) > 100

  SELECT Name, Count(Id)
    FROM Account
GROUP BY Name
  HAVING Count(Id) > 1

```
###### &nbsp;
#### HAVING을 사용할 때의 고려 사항
```sql
( OK )
  SELECT LeadSource, COUNT(Name)
    FROM Lead
GROUP BY LeadSource
  HAVING COUNT(Name) > 100and LeadSource > 'Phone'

( Error )
  SELECT LeadSource, COUNT(Name)
    FROM Lead
GROUP BY LeadSource
  HAVING COUNT(Name) > 100and City LIKE'San%'
```
###### &nbsp;
### TYPEOF (BATA)
TYPEOF는 다형성 관계를 포함하는 데이터를 쿼리 할 때 SOQL 쿼리의 SELECT 문에서 사용할 수있는 선택적 절입니다.

```sql
// 예제 1
SELECT Name 
FROM Account
WHERE CreatedById IN 
    (
    SELECT 
        TYPEOF Owner
            WHEN UserTHEN Id
            WHEN GroupTHEN CreatedById
        END
    FROM CASE
    )

// 예제 2
SELECT
    TYPEOF What
        WHEN Account THEN Phone
        ELSE Name
    END
FROM Event
WHERE CreatedById IN
    (
    SELECT CreatedById
    FROM Case
    )

// 예제 3
SELECT
    TYPEOF What
        WHEN Account THEN Phone, NumberOfEmployees
        WHEN Opportunity THEN Amount, CloseDate
        ELSE Name, Email
    END
FROM Event
```
###### &nbsp;
### FORMAT()
SELECT 절에 FORMAT을 사용하면 표준 및 사용자 정의 숫자, 날짜, 시간 및 통화 필드에 현지화 된 서식을 적용 할 수 있습니다.

```sql
// 예제 1
SELECT FORMAT(amount) Amt,
       format(lastModifiedDate) editDate
FROM Opportunity
```
Amt = "AED 1,500.000000 (USD 1,000.00)" <br/>
editDate = "7/2/2015 3:11 AM”

```sql
// 예제 2
SELECT Id, LastModifiedDate, FORMAT(LastModifiedDate) formattedDate FROM Account

SELECT amount,
       FORMAT(amount) FORMAT_Amt,
       convertCurrency(amount) convertedCurrency,
       FORMAT(convertCurrency(amount)) FORMAT_convertedCurrency
FROM Opportunity where id = '12345'
```
Amount = "34500000" <br/>
FORMAT_Amt= "KRW 34,500,000.00" <br/>
convertedCurrency ="34500000" <br/>
FORMAT_convertedCurrency = "KRW 34,500,000.00" <br/>

```sql
// 예제 3
SELECT FORMAT(MIN(closedate)) Amt FROM opportunity
```
###### &nbsp;
### FOR VIEW
SOQL 쿼리에서 FOR VIEW 절을 사용하여 개체를 마지막으로 본 시간에 대한 정보로 개체를 업데이트 할 수 있습니다.<br/>

* 이 절을 쿼리와 함께 사용하면 다음 두 가지 작업이 수행됩니다.
 + 검색된 레코드의 LastViewedDate 필드가 업데이트됩니다. 
 + 검색된 레코드에 대해 최근에 본 데이터를 반영하기 위해 레코드가 RecentlyViewed 개체에 추가됩니다.

 ```sql
 SELECT Name, ID FROM Contact  LIMIT1FORVIEW
 ```

###### &nbsp;
### FOR REFERENCE
FOR REFERENCE는 모바일 응용 프로그램 또는 사용자 정의 페이지에서와 같이 사용자 정의 인터페이스에서 <br/>
레코드를 참조 할 때 Salesforce에 알리기 위해 SOQL 쿼리에 사용할 수있는 선택적 절입니다.

* 이 절을 쿼리와 함께 사용하면 다음 두 가지 작업이 수행됩니다.
 + LastReferencedDate 필드는 검색된 레코드에 대해 업데이트됩니다.
 + 검색된 각 레코드에 대해 최근 참조 된 데이터를 반영하기 위해 레코드가 RecentlyViewed 개체에 추가됩니다.

 ```sql
 SELECT Name, ID FROM Contact  LIMIT1FOR REFERENCE
 ```
###### &nbsp;
### FOR UPDATE
Apex에서 경쟁 업데이트 및 기타 스레드 안전 문제를 방지하기 위해 FOR UPDATE를 사용하여 <br/>
sObject 레코드가 업데이트되는 동안 해당 레코드를 잠글 수 있습니다. <br/>
잠금을 사용하는 모든 SOQL 쿼리에서 ORDER BY 키워드를 사용할 수 없습니다.

```c#
Account [] accts = [SELECT Id FROM Account LIMIT 2 FOR UPDATE];
```
