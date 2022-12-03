---
title: RF 통신 모듈 제작
author: Park Nayoung
date: 2022-12-04 00:22:00 +0900
categories: [Exhibition,2022년]
tags: [post,parknayoung,jeonghyunkim]    
---

------------------------------------------
# RF 통신 모듈 제작

## 작품 배경
* 무선 통신은 전자 공학에 빠질 수 없는 요소임
* 해당 프로젝트를 통해서 회로 설계, 구조 탐구, 제어를 해보며 무선 통신을 경험해보고자 함
* RF 통신 모듈 제작을 통해 회로 설계, 아트웍, SMT 등의 전반 적인 과정을 이해하고자 하였음

## 작품 구현 알고리즘
1. Si4463칩셋
	+ PLL(출력신호 제어), LNA(신호 수신), PA(신호 송신), MODEM(신호 변복조 및 제어), 전원부 등 내장
	+ 위와 같은 송수신기와 제어기를 모두 내장하고 있는 Si4463 칩셋을 사용하여 RF 통신 모듈 제작
  
2. Class - E (CLE 매칭)
	+ swithching power amplifier 또는 switching power converter 의 역할을 함
	+ 10dbm이상의 출력 전력 전달
	+ drain voltage가 8v 를 넘지 않음
	+ 회로의 손실이 발생하지 않아 효율성이 높음
	+ 출력 전력이 입력 전력에 독립적(비선형적) : power converter
  
3. direct tie
	+ TX와 RX 경로가 직접적으로 연결 되어 있는 타입
	+ 하나의 안테나만으로 작동 가능
  
4. 아트웍 
	+ 해당 타입의 레이아웃 디자인 규칙에 따라 배치 후, via로 금속 주위를 둘러쌈
	+ 겉면을 gnd로 코팅함
  
-----
## 작품 과정 및 결과

* Class E (CLE), Direct Tie, 434 MHz, Pout = 16dBm, wire-wound Inductor
* 위의 조건에 맞게 PA 및 LNA 매칭값을 설정

<img src="./assets/img/post/2022-12-04-RF-communication-module/a.PNG" width="90%"> 

* easy eda 툴을 사용하여 회로 설계

<img src="./assets/img/post/2022-12-04-RF-communication-module/b.PNG" width="90%"> 

* 이를 바탕으로 아트웍 진행 후 발주

<img src="./assets/img/post/2022-12-04-RF-communication-module/c.JPG" width="90%"> 

* 제작된 마더 보드와 도더 보드 안테나를 납땜으로 연결

-----
## 보완해야할 과제

* 아트웍 품질이 부족하며 수치적으로 검증되지 않음
