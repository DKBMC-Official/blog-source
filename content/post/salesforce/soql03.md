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
title = "SOQL 기본문법(03)"

#설명
description = "날짜"

#프로필 사진 (사진: 자신의 한글 이름 입력 / 아바타이미지: 1~51 숫자 입력)
avatar = "차용진"

#비공개: false / 공개: true
draft = false


image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
comments = true
date = 2019-05-27T11:08:25+09:00
+++

<!-- 게시글 내용 -->

### 날짜 형식

Format | Format Syntax | Example
---|:---|:---
Date only | YYYY-MM-DD | 1999-01-01
Date, time, and time zone offset | YYYY-MM-DDThh:mm:ss+hh:mm <br/> YYYY-MM-DDThh:mm:ss-hh:mm <br/> YYYY-MM-DDThh:mm:ssZ | 1999-01-01T23:01:01+01:00 <br/> 1999-01-01T23:01:01-08:00 <br/> 1999-01-01T23:01:01Z

```sql
SELECT Id
  FROM Account
 WHERE CreatedDate > 2005-10-08T01:02:03Z
```
###### &nbsp;
### 날짜 문자

Date Literal | Example
---|:---
YESTERDAY | SELECT Id FROM Account WHERE CreatedDate = YESTERDAY
TODAY | SELECT Id FROM Account WHERE CreatedDate > TODAY
TOMORROW | SELECT Id FROM Opportunity WHERE CloseDate = TOMORROW
LAST_WEEK | SELECT Id FROM Account WHERE CreatedDate > LAST_WEEK
THIS_WEEK | SELECT Id FROM Account WHERE CreatedDate < THIS_WEEK
NEXT_WEEK | SELECT Id FROM Opportunity WHERE CloseDate = NEXT_WEEK
LAST_MONTH | SELECT Id FROM Opportunity WHERE CloseDate > LAST_MONTH
THIS_MONTH | SELECT Id FROM Account WHERE CreatedDate < THIS_MONTH
NEXT_MONTH | SELECT Id FROM Opportunity WHERE CloseDate = NEXT_MONTH
LAST_90_DAYS | SELECT Id FROM Account WHERE CreatedDate = LAST_90_DAYS
NEXT_90_DAYS | SELECT Id FROM Opportunity WHERE CloseDate > NEXT_90_DAYS
LAST_N_DAYS:n | SELECT Id FROM Account WHERE CreatedDate = LAST_N_DAYS:365
NEXT_N_DAYS:n | SELECT Id FROM Opportunity WHERE CloseDate > NEXT_N_DAYS:15
NEXT_N_WEEKS:n | SELECT Id FROM Opportunity WHERE CloseDate > NEXT_N_WEEKS:4
LAST_N_WEEKS:n | SELECT Id FROM Account WHERE CreatedDate = LAST_N_WEEKS:52
NEXT_N_MONTHS:n | SELECT Id FROM Opportunity WHERE CloseDate > NEXT_N_MONTHS:2
LAST_N_MONTHS:n | SELECT Id FROM Account WHERE CreatedDate = LAST_N_MONTHS:12
THIS_QUARTER | SELECT Id FROM Account WHERE CreatedDate = THIS_QUARTER
LAST_QUARTER | SELECT Id FROM Account WHERE CreatedDate > LAST_QUARTER
NEXT_QUARTER | SELECT Id FROM Account WHERE CreatedDate < NEXT_QUARTER
NEXT_N_QUARTERS:n | SELECT Id FROM Account WHERE CreatedDate < NEXT_N_QUARTERS:2
LAST_N_QUARTERS:n | SELECT Id FROM Account WHERE CreatedDate > LAST_N_QUARTERS:2
THIS_YEAR | SELECT Id FROM Opportunity WHERE CloseDate = THIS_YEAR
LAST_YEAR | SELECT Id FROM Opportunity WHERE CloseDate > LAST_YEAR
NEXT_YEAR | SELECT Id FROM Opportunity WHERE CloseDate < NEXT_YEAR
NEXT_N_YEARS:n | SELECT Id FROM Opportunity WHERE CloseDate < NEXT_N_YEARS:5
LAST_N_YEARS:n | SELECT Id FROM Opportunity WHERE CloseDate > LAST_N_YEARS:5
THIS_FISCAL_QUARTER | SELECT Id FROM Account WHERE CreatedDate = THIS_FISCAL_QUARTER
LAST_FISCAL_QUARTER | SELECT Id FROM Account WHERE CreatedDate > LAST_FISCAL_QUARTER
NEXT_FISCAL_QUARTER | SELECT Id FROM Account WHERE CreatedDate < NEXT_FISCAL_QUARTER
NEXT_N_FISCAL_QUARTERS:n | SELECT Id FROM Account WHERE CreatedDate < NEXT_N_FISCAL_QUARTERS:6
LAST_N_FISCAL_QUARTERS:n | SELECT Id FROM Account WHERE CreatedDate > LAST_N_FISCAL_QUARTERS:6
THIS_FISCAL_YEAR | SELECT Id FROM Opportunity WHERE CloseDate = THIS_FISCAL_YEAR
LAST_FISCAL_YEAR | SELECT Id FROM Opportunity WHERE CloseDate > LAST_FISCAL_YEAR
NEXT_FISCAL_YEAR | SELECT Id FROM Opportunity WHERE CloseDate < NEXT_FISCAL_YEAR
NEXT_N_FISCAL_YEARS:n | SELECT Id FROM Opportunity WHERE CloseDate < NEXT_N_FISCAL_YEARS:3
LAST_N_FISCAL_YEARS:n | SELECT Id FROM Opportunity WHERE CloseDate > LAST_N_FISCAL_YEARS:3