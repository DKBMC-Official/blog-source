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
title = "Lightning Web Component(1)"

#설명
description = "lcw 기초 정리"

#프로필 사진 (사진: 자신의 한글 이름 입력 / 아바타이미지: 1~51 숫자 입력)
avatar = "고봉훈"

#공개: false / 비공개: true
draft = false


image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
comments = true
date = 2019-11-27T16:52:12+09:00
+++

<!-- 게시글 내용 -->

### 1. Lightning Web Component 란?
-----------------------------------------

```Lightning Web Component( LCW )```는 HTML 및 최신 JavaScript를 사용하여 작성된 사용자 정의 HTML 요소 입니다.<br/>
LCW는 Aura Component 보다 성능이 좋고 개발이 더 쉽지만, LCW가 아직 Aura의 모든 기능을 지원하지는 않기 때문에,
Aura를 사용해야 할 수도 있습니다.

하지만, **지원되지 않는 기능이 필요하지 않다면 LCW를 선택해서 사용하는걸 권장하고 있습니다.**

LCW와 Aura의 가장 큰 차이점 중 하나는 Aura는 ```lightning:input``` 처럼 NameSpace와 componentName을 콜론으로 구분하지만,<br/>
LCW는 ```lightning-input```처럼 하이픈으로 구분 짓는다는 점입니다.

이 외에 LCW의 기본 소개를 보고 싶으시다면 <br/>
https://developer.salesforce.com/docs/component-library/documentation/lwc/lwc.get_started_introduction 를 참고 바랍니다.

<br/>

> [Lightning Web Component PlayGround](https://developer.salesforce.com/docs/component-library/tools/playground) 에서 LCW 코드를 작성하고 실행해 볼 수 있습니다. ( [PlayGround Document](https://developer.salesforce.com/docs/component-library/documentation/lwc/lwc.install_playground) )

<br/>

### 2. Component 폴더 만들기
----------------------------------------

컴포넌트는 기본적으로 폴더로 구분됩니다.<br/>
폴더와 파일 이름은 대문자와 밑줄을 포함하여 동일한 이름이어야 합니다.

{{<highlight batchFile>}}
    myComponent
        ├── myComponent.html
        ├── myComponent.js
        ├── myComponent.js-meta.xml
        ├── myComponent.css
        └── myComponent.svg
{{</highlight>}}

폴더의 명명 규칙은 다음과 같습니다.

- 소문자로 시작
- 영문,숫자 또는 밑줄만 포함되어야 함
- 공백을 포함 할 수 없음
- 밑줄로 끝날 수 없음
- 두 개의 연속 밑줄을 포함 할 수 없음
- 하이픈을 포함 할 수 없음

<br/>

### 3. Component HTML 파일
---------------------------------

컴포넌트의 html 파일은 &lt;template&gt; 태그 안에 HTML 요소를 구성하는 방식입니다.<br/>
또한, 만약 app.html에 myComponent를 렌더링시키고 싶다면, ```<c-my-component>```의 형식으로 넣어줘야 합니다.<br/>

```<namespace-component-name>```( c는 기본 네임스페이스 입니다 )

{{<highlight html>}}
    <!-- app.html -->
    <template>
        <c-my-component></c-my-component>
    </template>
{{</highlight>}}

<br/>

### 4. Component JavaScript 파일
--------------------------------------

{{<highlight JavaScript>}}
    // myComponent.js
    import { LightningElement } from 'lwc';
    export default class MyComponent extends LightningElement {
        ....
    }
{{</highlight>}}

위의 코드 처럼, 클래스 이름을 myComponent가 아닌 첫 글자를 대문자로 한 MyComponent를 써주셔야 합니다.

<br/>

### 5. Component Config 파일
-------------------------------------

모든 컴포넌트에는 Config 파일이 있어야 합니다. Config 파일은 Lightning App Builder 및 Community Builder의 디자인 구성을 포함해서, 컴포넌트의 메타 데이터 값을 정의합니다.

명명 규칙은 ```componentName.js-meta.xml``` 입니다.
{{<highlight html>}}
    <?xml version="1.0" encoding="UTF-8"?>
    <LightningComponentBundle xmlns="http://soap.sforce.com/2006/04/metadata">
        <apiVersion>45.0</apiVersion>
        <isExposed>false</isExposed>
    </LightningComponentBundle>
{{</highlight>}}

> Config 파일을 컴포넌트에 포함하지 않으면, 컴포넌트를 Org에 Deploy할 때 ``` Cannot find Lightning Component Bundle ... ``` 같은 오류가 발생합니다.

<br/>

### 6. Component Css 파일
-----------------------------

1. Css 파일의 이름과 컴포넌트의 이름이 같아야 합니다. 그러면 스타일 시트가 자동으로 해당 컴포넌트에 적용됩니다.
2. 각 컴포넌트는 하나의 스타일 시트만 가질 수 있습니다.
3. 컴포넌트의 스타일 시트에 정의 된 스타일의 범위는 해당 컴포넌트로 제한 됩니다.

{{<highlight html>}}
    <!-- parent.html -->
    <template>
        <h1>Parent Text</h1>
        <c-child></c-child>
    </template>
{{</highlight>}}

{{<highlight css>}}
    /* parent.css */
    h1 {
        font-size: 30px;
        color: red;
    }
{{</highlight>}}

{{<highlight html>}}
    <!-- child.html -->
    <template>
        <h1>Child Text</h1>
    </template>
{{</highlight>}}

위와 같이 구성을 할 경우, parent.css에 정의된 h1 태그의 스타일은 parent 컴포넌트에 child 컴포넌트가 포함되어 있더라도, child 컴포넌트에 영향을 끼치지 않습니다.
