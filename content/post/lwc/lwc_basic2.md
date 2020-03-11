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
title = "Lightning Web Component(2)"

#설명
description = "lcw 기초 정리"

#프로필 사진 (사진: 자신의 한글 이름 입력 / 아바타이미지: 1~51 숫자 입력)
avatar = "고봉훈"

#공개: false / 비공개: true
draft = false


image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
comments = true
date = 2019-12-10T14:18:57+09:00
+++

<!-- 게시글 내용 -->

### 1. 템플릿 데이터 바인딩
-----------------------------------------

1. HTML Template에서는 ```{property}```를 중괄호 안에 넣어야합니다.
2. Aura 컴포넌트와 달리 LWC에서는 HTML Template 내의 property에 수식 적용이 불가능 합니다.
3. 속성 값을 계산을 해서 가져오려면 JavaScript의 getter, setter를 이용하시면 됩니다.<br/>
```get property() { return ... }```
4. 오브젝트로부터 property에 접근할 때는 기존과 같이 dot notation을 이용하시면 됩니다.<br/>
```person.firstName``` , ```person[2].name['BongHoon']```


{{<highlight html>}}
    <!-- test.html -->
    <template>
        제 이름은 {myName} 입니다.
    </template>
{{</highlight>}}

{{<highlight javaScript>}}
    /* test.js */
    import { LightningElement } from 'lwc';
    export default class Test extends LightningElement {
        myName = "고봉훈";
    }
{{</highlight>}}


> 참고 : 컴포넌트가 리렌더링되면 템플릿에 사용된 모든 표현식이 재평가됩니다.

<br/>

### 2. 입력값에 따른 데이터 바인딩
---------------------------------------

{{<highlight html>}}
    <!-- inputTest.html -->
    <template>
        <p>제 이름은 {myName} 입니다.</p>
        <lightning-input label="Name" value={myName} onchange={handleChange}></lightning-input>
    </template>
{{</highlight>}}

{{<highlight javaScript>}}
    /* inputTest.js */
    import { LightningElement, track } from 'lwc';
    export default class InputTest extends LightningElement {
        @track myName = "고봉훈";

        handleChange(e) {
            this.myName = e.target.value;
        }
    }
{{</highlight>}}

<br/>

### 3. 수식 대신 Getter 사용
----------------------------------

속성 값을 계산하려면 getter를 사용해야합니다.

js에서 getter 정의<br/>
```get propertyName() { ... }```

템플릿에서 getter에 접근<br/>
```<p> {propertyName} </p>```


ex) 입력된 이름을 대문자로 바꾸는 예제
{{<highlight html>}}
    <!-- upperCase.html -->
    <template>
        <lightning-input name="firstName" label="First Name" onchange={handleChange}></lightning-input>
        <lightning-input name="lastName" label="Last Name" onchange={handleChange}></lightning-input>
        <p>{uppercasedName}</p>
    </template>
{{</highlight>}}

{{<highlight javaScript>}}
    /* upperCase.js */
    import { LightningElement, track } from 'lwc';

    export default class UpperCase extends LightningElement {
        @track firstName = '';
        @track lastName = '';

        handleChange(e) {
            const field = e.target.name;
            if(field === 'firstName') {
                this.firstName = e.target.value;
            }
            else {
                this.lastName = e.target.value;
            }
        }

        get uppercasedName() {
            return `${this.firstName} ${this.lastName}`.toUpperCase();
        }
    }
{{</highlight>}}

> 이번 포스트에서 쓰인 ```@track``` 속성은 해당 컴포넌트의 Private 속성이면서 값이 바뀌면 템플릿으로 리렌더링되는 반응성 속성입니다.
@track 속성을 쓰지 않고 위의 코드처럼 실행하면, 속성 자체의 값은 바뀌지만 템플릿에서 출력되는 값은 바뀌지 않게 됩니다.<br/>
Public 속성이면서 반응성 속성인 ```@api```도 있는데, 뒤에서 자세히 다뤄보겠습니다.

<br/>

### 4. 조건부 렌더링
---------------------------------

조건부 렌더링을 하기 위해서는 &lt;template&gt; 안에 또 &lt;template&gt; 를 만들거나, 일반 태그를 추가하고 ```if:true|false={...}``` 를 넣어 주시면 됩니다.<br/>
그러면 ```if:true``` 일 경우에는 {...} 값이 true일 경우 렌더링 되고, ```if:false``` 일 경우에는 {...} 값이 false일 경우 렌더링 됩니다.

{{<highlight html>}}
    <!-- rendering.html -->
    <template>
        <lightning-card title="조건부 렌더링 Test" icon-name="custom:custom14">
            <lightning-input type="checkbox" label="Show Detail" onchange={handleChange}></lightning-input>
            <template if:true={isVisible}>
                <div class="slds-m-vertical_medium">
                    SHOW!
                </div>
            </template>
        </lightning-card>
    </template>
{{</highlight>}}

{{<highlight javaScript>}}
    /* rendering.js */
    import { LightningElement, track } from 'lwc';

    export default class Rendering extends LightningElement {
        @track isVisible = false;

        handleChange(e) {
            this.isVisible = e.target.checked;
        }
    }
{{</highlight>}}

<br/>

### 5. 목록 반복 렌더링
--------------------------------

반복 렌더링을 하는 방법은 ```for:each```와 ```iterator:iteratorName``` 두 가지 방법이 있습니다.<br/>
어떤 반복문을 사용하는지에 관계없이 ```key``` 값을 넣어서 고유 ID를 할당해 줘야합니다.<br/>
목록이 변경되면 ```key```를 사용하여 변경된 항목만 다시 렌더링 하여 템플릿 성능을 최적화 하며, 실행시 DOM에 반영되지 않습니다.<br/>
또한 ```key``` 값을 넣지 않으면 오류가 발생합니다.

#### for:each

for:each={array}로 반복 시킬 데이터를 가져오고,  for:item="..."를 통해 각 아이템에 접근합니다.<br/>
for:index="..."로 인덱스 값을 받아올 수도 있습니다. 하지만 index값을 key값으로 설정할 수 없습니다.

{{<highlight html>}}
    <!-- contacts.html -->
    <template>
        <ul>
            <template for:each={contacts} for:item="contact" for:index="index">
                <li key={contact.Id}>
                    {index} : {contact.Name} , {contact.Title}
                </li>
            </template>
        </ul>
    </template>
{{</highlight>}}


{{<highlight javaScript>}}
    <!-- contacts.js -->
    import { LightningElement, track } from 'lwc';

    export default class Contacts extends LightningElement {
        @track contacts = [
            {
                Id : 1,
                Name : "BongHoon",
                Title : "Publisher"
            },
            {
                Id : 2,
                Name : "Test",
                Title : "LWC"
            }
        ];
    }
{{</highlight>}}

<br/>

#### iterator

iterator:iteratorName={array} 로 반복 시킬 데이터를 가져오고,<br/> 
iteratorName에 아래의 속성을 붙여 각 아이템을 가져옵니다.

 - value : 목록에 있는 항목의 값. ```iteratorName.value.propertyName```
 - index : 목록의 인덱스 값
 - first : 이 항목이 목록의 첫 번째 항목인지를 나타내는 Boolean 값
 - last : 이 항목이 목록의 마지막 항목인지를 나타내는 Boolean 값

{{<highlight html>}}
    <template>
        <ul>
            <template iterator:itr={contacts}>
                <li key={itr.value.Id}>
                    <div if:true={itr.first} class="list-first">First</div>
                    {itr.value.Name} , {itr.value.Title}
                    <div if:true={itr.last} class="list-last">Last</div>
                </li>
            </template>
        </ul>
    </template>
{{</highlight>}}

<br/>

### 6. 여러 템플릿 렌더링
-------------------------------------

{{<highlight javaScript>}}
    // multipleRender.js
    import { LightningElement, track  } from 'lwc';
    import templateOne from './templateOne.html';
    import templateTwo from './templateTwo.html';

    export default class MultipleRender extends LightningElement {
        @track templateOne = true;

        render() {
            return this.templateOne ? templateOne : templateTwo;
        }

        switchTemplate() {
            this.templateOne = this.templateOne = true ? false : true;
        }
    }
{{</highlight>}}

{{<highlight html>}}
    <!-- templateOne.html -->
    <template>
        <lightning-card title="Template One">
            <div>
                Template One
            </div>
            <p class="margin-vertical-small">
                <lightning-button label="Switch Templates" 
                    onclick={switchTemplate}>
                </lightning-button> 
            </p>
        </lightning-card>
    </template>
{{</highlight>}}

{{<highlight html>}}
    <!-- templateOne.html -->
    <template>
        <lightning-card title="Template Two">
            <div>
                Template Two
            </div>
            <p class="margin-vertical-small">
                <lightning-button label="Switch Templates" 
                    onclick={switchTemplate}>
                </lightning-button> 
            </p>
        </lightning-card>
    </template>
{{</highlight>}}

> 참고 : 컴포넌트에서 ```render()```를 이용하면 template 안의 내용을 전부 render()의 return 값으로 바꿔버립니다. render()를 이용하는 것 보다 ```if:true|false```로 렌더링 하는 것이 더 일반적입니다.