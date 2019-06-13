+++
#카테고리
categories = [
    "LC Front",
]

#작성자
authors = [
    "Jieun-Lee",
]

#제목
title = "Quick Action Modal"

#설명
description = "Custom Quick Action Modal"

#프로필 사진 (사진: 자신의 한글 이름 입력 / 아바타이미지: 1~51 숫자 입력)
avatar = "8"

#공개: false / 비공개: true
draft = false


image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
comments = true
date = 2019-06-07T14:53:34+09:00
+++

<!-- 게시글 내용 -->
### 1. Sample Setting - height : 250px / header = exist / footer = exist

---

#### - Preview
![QuickactionModalDefault](https://user-images.githubusercontent.com/50821963/59089950-5a776180-8946-11e9-9883-9ef13cd6a50c.jpg "QuickactionModalDefault")

---

#### - Component   
{{< highlight html >}}    
<aura:component access="global" implements="force:hasRecordId,force:hasSObjectName,force:lightningQuickActionWithoutHeader">
    <aura:handler name="init" value="{!this}" action="{!c.init}" /> 
    <aura:attribute name="isMobile" type="Boolean"/>

    <div class="{!v.isMobile ? 'quick-modal is-mobile' :'quick-modal' }">
        <section class="modal slds-modal slds-fade-in-open">
            <div class="slds-modal__container modal__container">
                <header class="slds-modal__header">
                    <h2 id="modal-heading-01" class="slds-text-heading_medium slds-hyphenate">Quick Action Modal</h2>
                </header>
                <div class="slds-modal__content modal__content slds-p-around_medium slds-scrollable_y" id="modal-content-id-1">
                    <div class="modal_body">
                        <p>Confirm ?</p>
                    </div>
                </div>
                <footer class="slds-modal__footer">
                    <button class="slds-button slds-button_neutral" disabled="{!v.isLoaded}" onclick="{!c.handleCancel}">
                        Cancel
                    </button>
                    <button class="slds-button slds-button_brand" disabled="{!v.isLoaded}" onclick="{!c.handleConfirm}">
                        Confirm
                    </button>
                </footer>
            </div>
        </section>
    </div>
</aura:component>
{{< /highlight >}}
<br>
#### - Controller
{{< highlight javascript >}}
({
    init : function(component, event, helper) {
        var device = $A.get("$Browser.formFactor");
        if(device != "DESKTOP"){
            component.set('v.isMobile',true);
        }    
    },
    handleCancel : function(component, event, helper){
        $A.get("e.force:closeQuickAction").fire();
    }
})
{{< /highlight >}}
<br>
#### - Style
{{< highlight css >}}
/*modal의 높이를 250px로 설정*/
.THIS.quick-modal{
    width: calc(100% + 4rem);/*퀵액션모달자체의 패딩 값(left,right 2rem씩)을 더한다*/
    padding:0;
    margin:0;
    margin-left: -2rem;/*퀵액션모달자체의 패딩(left 2rem)만큼 뺀다*/
    margin-top:-1rem;/*퀵액션모달자체의 패딩(top 1rem) 만큼 뺀다*/
}
.THIS .modal{
    position: unset;
}
.THIS .modal__container{ 
    width: 100%;
    max-width:100%;
    height: 100%;
    margin: 0;
    padding: 0; 
}
.THIS .modal__content{
    height: calc(250px - 116.4px);/*설정모달의 높이에서 해당컴포넌트 모달의 헤더,푸터의 높이를 뺀다*/
}

.THIS .modal_body{
    width:100%;
    height:100%;
    display:flex;/*모달 바디 영역의 텍스트가 센터로 오게하기 위한 설정(옵션)*/
    justify-content:center;
    align-items:center;
}
.THIS .slds-modal__footer{
    width: 100%;
    margin-bottom: -57.6px;/*푸터 자신의 높이 값 만큼 뺀다*/
}
.THIS.is-mobile{
    height: 99.6vh;
    width: 100vw;
    margin: 0;
    padding: 0;
    position: fixed;
    left: 0;
    top: 0;
}
.THIS.is-mobile .modal{
    height: 100%;
    margin-top: 0;
    width:100%;
}
.THIS.is-mobile .modal__content{
    height: calc(100vh - 121.2px);/*화면전체 높이에서 해당컴포넌트 모달의 헤더,푸터의 높이를 뺀다*/
}

.THIS.is-mobile .slds-modal__footer{
    width: 100%;
    margin-bottom: 0px;
}
{{< /highlight >}}
<br>

### 2. Sample Setting - height : 250px / header = none / footer = exist
---

#### - Preview
![QuickactionModalDefault](https://user-images.githubusercontent.com/50821963/59089999-7da21100-8946-11e9-99d8-4cb6de6abd4c.jpg "QuickactionModalDefault")

---

#### - Component
{{< highlight html >}}
<aura:component access="global" implements="force:hasRecordId,force:hasSObjectName,force:lightningQuickActionWithoutHeader">
    <aura:handler name="init" value="{!this}" action="{!c.init}" /> 
    <aura:attribute name="isMobile" type="Boolean"/>

    <div class="{!v.isMobile ? 'quick-modal is-mobile' :'quick-modal' }">
        <section class="modal slds-modal slds-fade-in-open">
            <div class="slds-modal__container modal__container">
                <div class="slds-modal__content modal__content slds-p-around_medium slds-scrollable_y" id="modal-content-id-1">
                    <div class="modal_body">
                        <p>Confirm ?</p>
                    </div>
                </div>
                <footer class="slds-modal__footer">
                    <button class="slds-button slds-button_neutral" disabled="{!v.isLoaded}" onclick="{!c.handleCancel}">
                        Cancel
                    </button>
                    <button class="slds-button slds-button_brand" disabled="{!v.isLoaded}" onclick="{!c.handleConfirm}">
                        Confirm
                    </button>
                </footer>
            </div>
        </section>
    </div>
</aura:component>
{{< /highlight >}}
<br>
#### - Controller
{{< highlight javascript >}}
({
    init : function(component, event, helper) {
        var device = $A.get("$Browser.formFactor");
        if(device != "DESKTOP"){
            component.set('v.isMobile',true);
        }    
    },
    handleCancel : function(component, event, helper){
        $A.get("e.force:closeQuickAction").fire();
    }
})
{{< /highlight >}}
<br>
#### - Style
{{< highlight css >}}
/*modal의 높이를 250px로 설정*/
.THIS.quick-modal{
    width: calc(100% + 4rem);/*퀵액션모달자체의 패딩 값(left,right 2rem씩)을 더한다*/
    padding:0;
    margin:0;
    margin-left: -2rem;/*퀵액션모달자체의 패딩(left 2rem)만큼 뺀다*/
    margin-top:-1rem;/*퀵액션모달자체의 패딩(top 1rem) 만큼 뺀다*/
}
.THIS .modal{
    position: unset;
}
.THIS .modal__container{ 
    width: 100%;
    max-width:100%;
    height: 100%;
    margin: 0;
    padding: 0; 
}
.THIS .modal__content{/*헤더가 없을 시*/
    height: calc(250px - 57.6px);/*설정모달의 높이에서 해당컴포넌트 모달 푸터의 높이를 뺀다*/
}
.THIS .modal_body{
    width:100%;
    height:100%;
    display:flex;/*모달 바디영역의 텍스트가 센터로 오게하기 위한 설정(옵션)*/
    justify-content:center;
    align-items:center;
}
.THIS .slds-modal__footer{
    width: 100%;
    margin-bottom: -57.6px;/*푸터 자신의 높이 값 만큼 뺀다*/
}
.THIS.is-mobile{
    height: 100vh;
    width: 100vw;
    margin: 0;
    padding: 0;
    position: fixed;
    left: 0;
    top: 0;
}
.THIS.is-mobile .modal{
    height: 100%;
    margin-top: 0;
    width:100%;
}
.THIS.is-mobile .modal__content{/*헤더가 없을 시*/
    height: calc(100vh - 64.9px);/*화면전체 높이에서 해당컴포넌트 모달 푸터의 높이를 뺀다*/
}
.THIS.is-mobile .slds-modal__footer{
    width: 100%;
    margin-bottom: 0px;
}
{{< /highlight >}}