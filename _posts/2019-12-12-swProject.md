---
layout: post
title:  "Sw Project"
date:   2019-12-12
excerpt: "Sw project"
project: true
tag:
- adoxx 
- project
comments: true
---

adoxx 와 아두이노를 통해 아두이노 자동차를 이용하여 배달 알고리즘을
구축하는 것이 이번 프로젝트의 목표입니다. 

## Diaram

{% capture images %}
https://user-images.githubusercontent.com/49140815/139758103-6bb91316-059c-4195-ad3b-e7f5579300ab.PNG
{% endcapture %}
{% include gallery images=images caption="Data flow Diagram" cols=1 %}

위 사진에서 저는 adoxx를 통해 서버와 연동시켜 배달해야하는 위치와 차로 부터의 실사간 위치 혹은 
이외의 차로부터 오는 연락들을 받아 상점 혹은 배달주인에게 전파하는 역할을 구현하는 담당이였습니다

{% capture images %}
https://user-images.githubusercontent.com/49140815/139769093-0da2b129-29b7-47da-8a26-c5aafc8b56d7.PNG
{% endcapture %}
{% include gallery images=images caption="State and Squence Diagram" cols=1 %}

위 사진은 저희가 구현할 알고리즘의 State, Squence 다이어그램입니다

## Trial Run

<iframe width="560" height="315" src="//www.youtube.com/embed/Zxh9BdxJVA8" frameborder="0"> </iframe>

위 동영상은 adoxx로 코딩을 한 이후 입력받은 정보로 자동차가 이동하는 위치를 표시하는 
동작을 나타낸 동영상입니다

<iframe width="560" height="315" src="//www.youtube.com/embed/DNriPSghztQ" frameborder="0"> </iframe>

위 동영상은 실제 정보를 입력받아 그에 맞게 배달을 하는 영상입니다

원래는 자동차가 앞뒤뿐 아니라 좌우도 배달하는 알고리즘을 작성하는 것이 목표였으나
지원받은 자동차가 결함이 있어 좌우로 이동을 할수가 없어 앞뒤로만 움직이는 알고리즘으로
운용하였습니다