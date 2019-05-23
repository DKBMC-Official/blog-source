+++
tags = [
    "DK BMC"
]
categories = [
    "Hugo Setting",
]
authors = [
    "고봉훈"
]
image = "/img/about-bg.jpg" #optional image - "/img/about-bg.jpg" is the default
title = "DK BMC 기술블로그"
description = "나의 첫번 째 포스트"
draft = false
comments = true
+++

<h2>1. DKBMC</h2>
<p>
    배달의민족 앱의 각 페이지들의 정보는 API를 호출해서 데이터를 가져온 후 사용자에게 정보를 노출하게 되는데 해당 페이지 정보의 데이터 고도화 및 신규 기능으로 인하여 API 주소가 변경 되어야 할 경우가 있습니다.
    그래서 개발중인 프로젝트 테스트를 위해 API 주소 변경에 대한 요청이 발생되는데 그럴때 마다 프로젝트 환경에 맞춰서 앱에서 API들을 재설정을 해서 배포하게 됩니다.
    문제는 다수의 프로젝트에서 API 주소 변경 요청 건들이 온다면? 각각의 프로젝트를 위해 API 정보들을 재설정 후 빌드 배포하는 행위를 반복적으로 하게 될겁니다.
    그래서 앱의 API 정보 변경이 필요하면 앱 빌드와 배포 없이 간단한 설정으로 API 정보 변경을 할 수 있도록 하여 개발자들의 업무 생산성이 나아지는 방법이 필요했습니다.
</p>
