+++
#카테고리
categories = [
    "LWC",
]

#작성자
authors = [
    "고봉훈",
]

#제목
title = "Lightning Web Component(3)"

#설명
description = "lcw 기초 정리"

#프로필 사진 (사진: 자신의 한글 이름 입력 / 아바타이미지: 1~51 숫자 입력)
avatar = "고봉훈"

#공개: false / 비공개: true
draft = false


image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
comments = true
date = 2019-12-17T10:13:40+09:00
+++

<!-- 게시글 내용 -->

### 1. JavaScript Property 이름
----------------------------

JavaScript에서의 property의 이름은 ```Camel Case```, HTML에서의 property 이름은 ```Kebab Case```로 써야합니다.<br/>
예를들어, JavaScript에서 property 이름이 **itemName** 이라면, HTML에서는 **item-name**으로 매핑됩니다.

아래의 문자로 속성 이름을 시작하면 안됩니다.

 - on (예: onClick)
 - aira (예: ariaDescribedby)
 - data (예: dataProperty)

아래의 예약어를 속성 이름으로 사용하면 안됩니다.

 - slot
 - part
 - is

 <br/>

### 2. JavaScript에서 HTML 전역 속성에 접근
-----------------------------------------------

 일부 HTML 전역 속성은 LWC Camel Case 및 Kebab Case 규칙을 따르지 않습니다.<br/>
 이러한 HTML 전역 속성 중 하나에 대한 getter, setter를 작성하는 경우 아래 목록을 참고하세요.

|  <center>HTML 전역 속성</center> |  <center>JAVASCRIPT의 속성</center> |
|:--------|:--------|
|accesskey | accessKey |
|bgcolor | bgColor |
|colspan | colSpan |
|contenteditable|contentEditable|
|crossorigin|crossOrigin|
|datetime|dateTime|
|for|htmlFor|
|formaction|formAction|
|ismap|isMap|
|maxlength|maxLength|
|minlength|minLength|
|novalidate|noValidate|
|readonly|readOnly|
|rowspan|rowSpan|
|tabindex|tabIndex|
|usemap|useMap|

<br/>

### 3. JavaScript로 ARIA 속성에 접근
-------------------------------------------

ARIA는 웹 보조기기를 지원하는 속성입니다. JavaScript로 이 속성에 접근하려면 Camel Case를 사용합니다.<br/>
예를들어, **aria-checked**에 접근하려면 **ariaChecked**를 사용

<br/>

### 4. 반응성 속성
--------------------------------

#### 1. Public Property

Public Property를 노출시키려면 ```@api```를 사용해야합니다.<br/>
마크업에서 부모 컴포넌트는 자식 컴포넌트의 Public Property에 접근할 수 있습니다.<br/>
Public Property는 반응 속성입니다. 반응 속성은 값이 변경되면 컴포넌트가 리렌더링 되며, 템플릿에 사용 된 모든 표현식이 재평가됩니다.

```@api```를 사용하려면 아래처럼 lwc에서 import 시켜야합니다.
{{<highlight javaScript>}}
    import { LightningElement, api } from 'lwc';
{{</highlight>}}


예제 ) 
{{<highlight javaScript>}}
    // todoItem.js
    import { LightningElement, api } from 'lwc';
    export default class TodoItem extends LightningElement {
        @api itemName = 'New Item';
    }
{{</highlight>}}

{{<highlight html>}}
    <!-- todoItem.html -->
    <template>
        <div class="view">
            <label>{itemName}</label>
        </div>
    </template>
{{</highlight>}}

{{<highlight html>}}
    <!-- todoApp.html -->
    <template>
        <div class="listing">
            <c-todo-item item-name="Milk"></c-todo-item>
            <c-todo-item item-name="Bread"></c-todo-item>
        </div>
    </template>
{{</highlight>}}

{{<highlight javaScript>}}
    // todoApp.js
    const myTodo = this.template.querySelector('c-todo-item');
    myTodo.itemName // New Item
{{</highlight>}}

<br/>

#### 2. Tracked(Private Reactive) Property
Tracked Property는 Private Reactive Property라고도 부릅니다.<br/>
Private 속성의 값을 추척하고 변경 될 때 컴포넌트를 리렌더링하려면 ```@track```속성을 사용하세요.

@api와 달리 @track속성은 비공개 속성입니다.

> @track 속성은 과도하게 사용하지 말고, 속성 값이 변경 될 때 컴포넌트를 리렌더링 시켜야하는 경우에만 사용하는걸 권장합니다.

```@track```를 사용하려면 아래처럼 lwc에서 import 시켜야합니다.
{{<highlight javaScript>}}
    import { LightningElement, track } from 'lwc';
{{</highlight>}}

예제 )
{{<highlight html>}}
    <!-- simpleTrack.html -->
    <template>
        <p>{itemName}</p>
        <lightning-button label="Change Item" onclick={handleClick}></lightning-button>
    </template>
{{</highlight>}}

{{<highlight javaScript>}}
    // simpleTrack.js
    import { LightningElement, track } from 'lwc';

    export default class App extends LightningElement {
        @track itemName = 'milk';

        handleClick(){
            this.itemName = 'bread';
            console.log('itemName is ' + this.itemName);
        }
    }
{{</highlight>}}

위의 예제에서 컴포넌트가 처음 렌더링 될 때는 itemName이 **milk**로 출력되지만, **Change Item**버튼을 클릭하면, itemName이 **bread**로 바뀌고 컴포넌트가 리렌더링되어 **bread**가 출력됩니다.<br/>
만약 itemName 앞에 ```@track```이 없다면, console에서는 **bread**로 찍히지만, 컴포넌트가 리렌더링 되지 않아 템플릿에서는 여전히 **milk**로 출력됩니다.

<br/>

