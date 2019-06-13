+++
#카테고리
categories = [
    "Apex Trigger",
]

#작성자
authors = [
    "Yong-Jin",
]

#제목
title = "Apex Trigger"

#설명
description = ""

#프로필 사진 (사진: 자신의 한글 이름 입력 / 아바타이미지: 1~51 숫자 입력)
avatar = "차용진"

#공개: false / 비공개: true
draft = false


image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
comments = true
date = 2019-05-29T10:35:20+09:00
+++

<!-- 게시글 내용 -->
### Trigger
트리거를 사용하여 Apex를 호출 할 수 있습니다. 
Apex 트리거를 사용하면 삽입, 업데이트 또는 삭제와 같은 Salesforce 레코드 변경 전후에 사용자 지정 작업을 수행 할 수 있습니다.

* 트리거는 다음 유형의 작업 전후에 실행되는 Apex 코드입니다.
    - insert
    - update
    - delete
    - merge
    - upsert
    - undelete

_Before 트리거_<br/>
레코드 값이 데이터베이스에 저장되기 전에 업데이트 또는 유효성 검사에 사용됩니다.

_After 트리거_<br/>
비동기 이벤트를 실행하는 등 다른 레코드의 변경 사항에 영향을 미치기 위해 사용<br/>
__동작하는 동안 레코드는 읽기 전용.__

###### &nbsp;
#### 구현 고려 사항

_upsert 트리거_<br/>
Insert - Beforce, After 또는 Update - Before, After<br/>

_merge 트리거_<br/>
    지워지는 레코드의 Delete - Before, After<br/>
    업데이트 되는 레코드의 Update - Before, After<br/>

_undeleted 트리거_<br/>
    휴지통에서 꺼낼 때 발생<br/>

트리거에서 외부 서비스 호출하는 Callout을 할 때는 비동기로 실행해야 합니다. <br/>
비동기 Callout을 만들려면 @future Method 와 같은 비동기 Apex를 사용하해야 합니다.

###### &nbsp;
#### Bulk Triggers (대량 트리거)

모든 트리거는 기본적으로 대량 트리거이며 한 번에 여러 레코드를 처리 할 수 있습니다.<br/>
한 번에 두 개 이상의 레코드를 처리하도록 항상 계획해야합니다.

- Data import
- Lightning Platform Bulk API calls
- Mass actions, such as record owner changes and deletes
- Recursive Apex methods and triggers that invoke bulk DML statements

```c#
trigger myAccountTrigger on Account (before insert, before update) {
    // Your code here
}
```
Apex 트리거가 성공적으로 완료되면 모든 데이터베이스 변경 사항이 자동으로 커밋됩니다. 
Apex 트리거가 성공적으로 완료되지 않으면 데이터베이스에 대한 모든 변경 사항이 롤백됩니다.

###### &nbsp;
#### Trigger Context Variables (트리거 문맥 변수)

Variable | Usage
---|:---
isExecuting |Returns true if the current context for the Apex code is a trigger, not a Visualforce page, a Web service, or an executeanonymous() API call.
isInsert | Returns true if this trigger was fired due to an insert operation, from the Salesforce user interface, Apex, or the API.
isUpdate | Returns true if this trigger was fired due to an update operation, from the Salesforce user interface, Apex, or the API.
isDelete | Returns true if this trigger was fired due to a delete operation, from the Salesforce user interface, Apex, or the API.
isBefore | Returns true if this trigger was fired before any record was saved.
isAfter | Returns true if this trigger was fired after all records were saved.
isUndelete | Returns true if this trigger was fired after a record is recovered from the Recycle Bin (that is, after an undelete operation from the Salesforce user interface, Apex, or the API.)
new | Returns a list of the new versions of the sObject records.<br/> This sObject list is only available in insert, update, and undelete triggers, and the records can only be modified in before triggers.
newMap | A map of IDs to the new versions of the sObject records.<br/> This map is only available in before update, after insert, after update, and after undeletetriggers.
old | Returns a list of the old versions of the sObject records.<br/>This sObject list is only available in update and delete triggers.
oldMap | A map of IDs to the old versions of the sObject records. his map is only available in update and delete triggers.
size | The total number of records in a trigger invocation, both old and new.

```c#
trigger myAccountTrigger on Account(before delete, before insert, before update,
                                    after delete, after insert, after update) {
if (Trigger.isBefore) {
    if (Trigger.isDelete) {
        // In a before deletetrigger, the trigger accesses the records that will be
        // deleted with the Trigger.old list.
        for (Account a : Trigger.old) {
            if (a.name != 'okToDelete') {
                a.addError('You can not delete this record!');
            }
        }
    } else {
    // Inbeforeinsertorbeforeupdate triggers, the trigger accesses the new records
    // with the Trigger.new list.
        for (Account a : Trigger.new) {
            if (a.name == 'bad') {
                a.name.addError('Bad name');
            }
    }
    if (Trigger.isInsert) {
        for (Account a : Trigger.new) {
            System.assertEquals('xxx', a.accountNumber);
            System.assertEquals('industry', a.industry);
            System.assertEquals(100, a.numberofemployees);
            System.assertEquals(100.0, a.annualrevenue);
            a.accountNumber = 'yyy';
        }
// If the triggerisnot a beforetrigger, it must be an aftertrigger.
} else {
    if (Trigger.isInsert) {
        List<Contact> contacts = new List<Contact>();
        for (Account a : Trigger.new) {       
            if(a.Name == 'makeContact') {
                contacts.add(new Contact (LastName = a.Name, AccountId = a.Id));
            }
        }
      insert contacts;
    }
  }
}}}
```
###### &nbsp;
#### Context Variable Considerations (문맥 변수 고려사항)

- trigger.new and trigger.old cannot be used in Apex DML operations.
- You can use an object to change its own field values using trigger.new, but only in before triggers. In all after triggers, trigger.new is not saved, so a runtime exception is thrown.
- trigger.old is always read-only.
- You cannot delete trigger.new.

###### &nbsp;
#### Triggers and Merge Statements

* Merge 시 이벤트 순서
    1. Before delete 트리거 실행
    2. Merge 작업으로 필요한 레코드 삭제, 새 레코드를 자식 레코드에 할당하고, 삭제 된 레코드에 MasterRecordId 필드를 설정
    3. After delete 트리거가 실행
    4. 시스템은 마스터 레코드에 필요한 특정 갱신을 수행하고 일반 업데이트 트리거 적용

###### &nbsp;
#### Trigger Exceptions (트리거 예외)

트리거는 레코드 또는 필드에서 addError () 메서드를 호출하여 DML 작업이 발생하는 것을 방지하는 데 사용할 수 있습니다.
insert, update 트리거의 Trigger.new 레코드 및 delete 트리거의 Trigger.old 레코드에서 사용되는 경우 사용자 지정 오류 메시지가 응용 프로그램 인터페이스에 표시되고 기록됩니다.

###### &nbsp;
#### Trigger and Bulk Request Best Practices

```c#
( flawed pattern )
trigger MileageTrigger on Mileage__c (before insert, before update) {
   User c = [SELECT Id FROM User WHERE mileageid__c = Trigger.new[0].id];
}

( flawed pattern )
trigger MileageTrigger on Mileage__c (before insert, before update) {
   for(mileage__c m : Trigger.new){
      User c = [SELECT Id FROM user WHERE mileageid__c = m.Id];
   }
}

( correct pattern )
Trigger MileageTrigger on Mileage__c (before insert, before update) {
   Set<ID> ids = Trigger.newMap.keySet();
   List<User> c = [SELECT Id FROM user WHERE mileageid__c in:ids];
}
```